
## 재은이가 준 소중한 코드



```bash
#!/bin/bash
# main_script.sh

# ==============================
# Docker 네트워크 목록 출력
# ==============================
echo "사용 가능한 Docker 네트워크 목록:"
NETWORKS=($(docker network ls --format "{{.Name}}")) # 네트워크 이름 배열로 저장
for i in "${!NETWORKS[@]}"; do
  echo "$((i+1)). ${NETWORKS[$i]}" # 줄바꿈 보장
done

# 사용자로부터 네트워크 선택 및 확인
while true; do
  read -p "사용할 Docker 네트워크 번호를 입력하세요: " NETWORK_NUMBER
  NETWORK_NAME="${NETWORKS[$((NETWORK_NUMBER-1))]}"
  echo
  echo -n "선택한 네트워크가 맞습니까? $NETWORK_NAME (Y/N): "
  read CONFIRM
  if [[ "$CONFIRM" =~ ^[Yy]$ ]]; then
    break
  fi
done
echo

# ==============================
# 컨테이너 이름 입력 및 확인
# ==============================
while true; do
  read -p "생성할 컨테이너 이름을 입력하세요: " CONTAINER_NAME
  echo
  echo -n "입력한 컨테이너 이름이 맞습니까? $CONTAINER_NAME (Y/N): "
  read CONFIRM
  if [[ "$CONFIRM" =~ ^[Yy]$ ]]; then
    break
  fi
done
echo

# ==============================
# 서버명 추출 및 확인
# ==============================
SERVER_NAME="${CONTAINER_NAME##*-}"

while true; do
  echo -n "서버 이름은 '$SERVER_NAME'로 자동 추출됩니다. 사용하시겠습니까? (Y/N): "
  read CONFIRM
  if [[ "$CONFIRM" =~ ^[Yy]$ ]]; then
    break
  else
    read -p "사용할 서버명을 입력하세요: " SERVER_NAME
  fi
done
echo

# ==============================
# 네트워크 정보 가져오기
# ==============================
NETWORK_INFO=$(docker network inspect "$NETWORK_NAME" 2>/dev/null)

if [[ $? -ne 0 ]]; then
  echo "오류: 네트워크 정보를 가져오는 데 실패했습니다. 네트워크 이름을 확인해 주세요."
  exit 1
fi

# 서브넷 추출 (jq 대신 grep, awk 사용)
SUBNET=$(echo "$NETWORK_INFO" | grep -oP '"Subnet": "\K([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/[0-9]+)' )

if [ -z "$SUBNET" ]; then
  echo "오류: 서브넷을 추출할 수 없습니다."
  exit 1
fi

# 출력 내용 수정
echo "추출된 서브넷: $NETWORK_NAME ($SUBNET)"
echo

# ==============================
# IP 및 포트 번호 추출 (get_ip_port.sh 실행)
# ==============================
read IP PORT <<< $(./get_ip_port.sh "$NETWORK_NAME" "$SUBNET")

if [[ $? -ne 0 ]]; then
  echo "오류: IP 및 포트 번호를 가져오는 데 실패했습니다."
  exit 1
fi

# ==============================
# 버전 선택 로직 (select_version.sh의 내용 통합)
# ==============================

# version 디렉토리 절대 경로
VERSION_DIR="/home/ubuntu/work/version"

# 'v'로 시작하는 폴더만 가져오기, 자연스러운 정렬
folders=($(ls -d $VERSION_DIR/*/ | grep -E '/v[0-9]+\.[0-9]+\.[0-9]+.*' | sort -V))
counter=1

# 버전 목록 출력
echo "작업할 버전 목록:"
for folder in "${folders[@]}"; do
  folder_name=$(basename "${folder%/}")  # 폴더명 추출
  echo "$counter. $folder_name"
  ((counter++))
done
echo

# 사용자로부터 버전 번호 선택
while true; do
  read -p "작업할 버전 번호를 입력하세요: " VERSION_NUMBER
  echo

  # 입력값이 숫자인지 확인
  if ! [[ "$VERSION_NUMBER" =~ ^[0-9]+$ ]]; then
    echo "잘못된 입력입니다. 숫자만 입력해주세요."
    continue
  fi

  # 번호가 유효한지 확인
  if [[ $VERSION_NUMBER -gt 0 && $VERSION_NUMBER -le ${#folders[@]} ]]; then
    SELECTED_FOLDER="${folders[$((VERSION_NUMBER-1))]}"
    SELECTED_FOLDER_NAME=$(basename "${SELECTED_FOLDER%/}")
    
    # 선택한 폴더 내에 'server-0.0.1-SNAPSHOT.war' 파일이 있는지 확인
    if [[ -f "$SELECTED_FOLDER/server-0.0.1-SNAPSHOT.war" ]]; then
      # 선택된 버전만 출력
      SELECTED_VERSION="$SELECTED_FOLDER_NAME"
      echo
      echo -n "선택한 버전이 맞습니까? $SELECTED_VERSION (Y/N): "
      read CONFIRM
      if [[ "$CONFIRM" =~ ^[Yy]$ ]]; then
        break
      fi
    else
      echo "오류: $SELECTED_FOLDER_NAME 폴더 안에 'server-0.0.1-SNAPSHOT.war' 파일이 없습니다."
      continue
    fi
  else
    echo "잘못된 번호입니다. 다시 입력해주세요."
  fi
done
echo

# ==============================
# 최종 정보 출력
# ==============================
echo "--------------최종정보---------------"
echo "> 네트워크 : $NETWORK_NAME"
echo "> 컨테이너 명 : $CONTAINER_NAME"
echo "> 서버명 : $SERVER_NAME"
echo "> IP, PORT : $IP, $PORT"
echo "> VERSION : $SELECTED_VERSION"
echo "-------------------------------------"
echo

# ==============================
# 컨테이너 생성 스크립트 실행 및 리턴값 받기
# ==============================
CONTAINER_CREATION_RESULT=$(./create_container.sh "$NETWORK_NAME" "$CONTAINER_NAME" "$SERVER_NAME" "$IP" "$PORT")

if [[ $? -ne 0 ]]; then
    echo "오류: 컨테이너 생성에 실패했습니다."
    exit 1
fi

# 결과 출력
echo "컨테이너 생성 결과: $CONTAINER_CREATION_RESULT"
echo

# ==============================
# Nginx 설정 파일 생성 (create_nginx.sh 실행)
# ==============================
NginxCreationResult=$(./create_nginx.sh "$SERVER_NAME" "$IP")

if [[ $? -ne 0 ]]; then
    echo "오류: Nginx 설정 파일 생성에 실패했습니다."
    exit 1
fi

# Nginx 설정 결과 출력
echo "Nginx 설정 파일 생성 결과: $NginxCreationResult"
echo

# ==============================
# Nginx 설정 파일 검사 및 재시작
# ==============================
echo "Nginx 설정 파일을 검사합니다..."
sudo nginx -t
if [[ $? -eq 0 ]]; then
    echo "Nginx 설정 파일이 유효합니다."

    # Nginx 재시작
    echo "Nginx를 재시작합니다…"
    sudo nginx -s reload
else
    echo "오류: Nginx 설정 파일에 오류가 있습니다."
    exit 1
fi
echo

# 네트워크 포트 확인
echo "Nginx가 사용하는 포트 확인 중…"
netstat -nlpt | grep nginx
```

