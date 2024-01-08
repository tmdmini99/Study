
방법 1
```java
package Week2;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;

import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;
import java.sql.Connection;
import java.sql.DriverManager;

import java.sql.PreparedStatement;
import java.sql.Statement;

public class DataBaseInsert {
    public void main1() throws Exception {

        DbConnection dbConnection = new DbConnection();
        int pSize = 100;
        URL urls = new URL("https://openapi.gg.go.kr/TfcacdarM?KEY=e80e34c1a05d448497b437eb794dd5c8&pSize="+pSize); //공공 데이터 포탈에서 가져올시 키값과 내가 원하는 페이지 수 가져올 양을 url에 같이 적어야함
        HttpURLConnection con = (HttpURLConnection) urls.openConnection();


        con.setRequestProperty("Content-type", "application/json");


        StringBuffer sb = new StringBuffer();
        BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
        con.getInputStream();

        while (br.ready()) {

            sb.append(br.readLine());

        }
       // System.out.println(sb);

//        JSONParser parser = new JSONParser();
//        Object obj = parser.parse(sb.toString());
//        System.out.println(obj.getClass());
//       JSONObject page = (JSONObject) obj;
//       JSONArray jsonArr = (JSONArray) page.get("TfcacdarM");
//        System.out.println(jsonArr);
//       page = (JSONObject) jsonArr.get(0);
//        System.out.println(page);
//       jsonArr= (JSONArray) page.get("head");
//        System.out.println(jsonArr);
//       page = (JSONObject) jsonArr.get(0);
//        System.out.println(page);
        JSONParser parser = new JSONParser();
        JSONObject page = (JSONObject) parser.parse(sb.toString());
        JSONObject jsonArrObj = (JSONObject) ((JSONArray) page.get("TfcacdarM")).get(0);
        JSONObject head = (JSONObject) ((JSONArray) jsonArrObj.get("head")).get(0);
        int last = (Integer.parseInt(head.get("list_total_count").toString()));
        System.out.println(last);

        int pages = last/100;
        if(last%pSize>0){
            pages +=1;
        }

                Connection conn = dbConnection.getConnection();
                System.out.println("success");
                Statement stmt = conn.createStatement();//3. Query 준비
                System.out.println("pages"+pages);
                for(int i=1; i<=pages; i++){
							                    urls =new URL("https://openapi.gg.go.kr/TfcacdarM?KEY=e80e34c1a05d448497b437eb794dd5c8&pIndex="+i+"&pSize="+pSize);
                    con = (HttpURLConnection) urls.openConnection();
                    con.setRequestProperty("Content-type", "application/json");

                    sb = new StringBuffer();
                     br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
                    con.getInputStream();

                    while (br.ready()) {

                        sb.append(br.readLine());

                    }
                    parser = new JSONParser();
                    page = (JSONObject) parser.parse(sb.toString());
                    jsonArrObj = (JSONObject) ((JSONArray) page.get("TfcacdarM")).get(1);

                    for(int j=0; j< ((JSONArray) jsonArrObj.get("row")).size();j++){
                        JSONObject row =(JSONObject) ((JSONArray) jsonArrObj.get("row")).get(j);
                        //null 값 처리
                        String sig = (String) row.get("SIGUN_NM");
                        if(sig == null){
                            sig = "null";
                        }else{
                            sig = "'"+sig+"'";
                        }
                        String sql = " INSERT INTO op"
                                + " VALUES(" + sig + ", " + row.get("ACDNT_YY") + " , '" + row.get("ACDNT_DIV_NM") + "', " + row.get("MULTI_KNOWLG_DIV_NO")
                                + ", " + row.get("MULTI_KNOWLG_DIV_GROUP_NO") + ", " + row.get("LEGALDONG_CD_NO") + ", " + row.get("SPOT_NO") + ", '" + row.get("JURISD_POLCSTTN_NM") + "', " + row.get(" LOC_INFO")
                                + ", " + row.get("OCCUR_CNT") + ", " + row.get("CASLT_CNT") + ", " + row.get("DPRS_CNT") + ", " + row.get("SERINJRY_INDVDL_CNT") + ", " + row.get("SLTINJRY_INDVDL_CNT")
                                + ", " + row.get("INJURY_APLCNT_CNT") + ", " + row.get("LAT") + ", " + row.get("LOGT") + ", '" + row.get("MULTI_REGION_INFO")
                                + "') ";

                        //4. Query 실행 및 리턴
                        int res = stmt.executeUpdate(sql);
                        if(res > 0) {
                            System.out.println("입력 성공");
                        }else {
                            System.out.println("입력 실패");
                        }

                    }
                }
                dbConnection.disConnect(stmt,conn);


    }


    }


```



방법 2
```java
package Week2;  
  
import org.json.simple.JSONArray;  
import org.json.simple.JSONObject;  
import org.json.simple.parser.JSONParser;  
  
import java.io.*;  
import java.net.HttpURLConnection;  
import java.net.URL;  
import java.sql.Connection;  
import java.sql.DriverManager;  
  
import java.sql.PreparedStatement;  
import java.sql.Statement;  
  
public class DataBaseInsert2 {  
    public void main1() throws Exception {  
  
        DbConnection dbConnection = new DbConnection();  
        int pSize = 100;  
        URL urls = new URL("https://openapi.gg.go.kr/TfcacdarM?KEY=e80e34c1a05d448497b437eb794dd5c8&pSize="+pSize); //공공 데이터 포탈에서 가져올시 키값과 내가 원하는 페이지 수 가져올 양을 url에 같이 적어야함  
        HttpURLConnection con = (HttpURLConnection) urls.openConnection();  
  
  
        con.setRequestProperty("Content-type", "application/json");  
  
  
        StringBuffer sb = new StringBuffer();  
        BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));  
        con.getInputStream();  
  
        while (br.ready()) {  
  
            sb.append(br.readLine());  
  
        }    

        // System.out.println(sb);  
//        JSONParser parser = new JSONParser();  
//        Object obj = parser.parse(sb.toString());  
//        System.out.println(obj.getClass());  
//       JSONObject page = (JSONObject) obj;  
//       JSONArray jsonArr = (JSONArray) page.get("TfcacdarM");  
//        System.out.println(jsonArr);  
//       page = (JSONObject) jsonArr.get(0);  
//        System.out.println(page);  
//       jsonArr= (JSONArray) page.get("head");  
//        System.out.println(jsonArr);  
//       page = (JSONObject) jsonArr.get(0);  
//        System.out.println(page);  
        JSONParser parser = new JSONParser();  
        JSONObject page = (JSONObject) parser.parse(sb.toString());  
        JSONObject jsonArrObj = (JSONObject) ((JSONArray) page.get("TfcacdarM")).get(0);  
        JSONObject head = (JSONObject) ((JSONArray) jsonArrObj.get("head")).get(0);  
        int last = (Integer.parseInt(head.get("list_total_count").toString()));  
        System.out.println(last);  
  
        int pages = last/100;  
        if(last%pSize>0){  
            pages +=1;  
        }  
  
  
  
        jsonArrObj = (JSONObject) ((JSONArray) page.get("TfcacdarM")).get(1);  
//        JSONObject row = (JSONObject) ((JSONArray) jsonArrObj.get("row")).get(0);  
//        System.out.println("d"+((JSONArray) jsonArrObj.get("row")).size());  
//        //System.out.println("dd"+jsonArrObj);  
//        System.out.println(row);  
//        System.out.println(jsonArrObj.getClass());  
  
        Connection conn = dbConnection.getConnection();  
        System.out.println("success");  
        PreparedStatement stmt =null;  
        System.out.println("pages"+pages);  
        for(int i=1; i<=pages; i++){  
            urls =new URL("https://openapi.gg.go.kr/TfcacdarM?KEY=e80e34c1a05d448497b437eb794dd5c8&pIndex="+i+"&pSize="+pSize);  
            con = (HttpURLConnection) urls.openConnection();  
            con.setRequestProperty("Content-type", "application/json");  
  
            sb = new StringBuffer();  
            br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));  
            con.getInputStream();  
  
            while (br.ready()) {  
  
                sb.append(br.readLine());  
  
            }  
            parser = new JSONParser();  
            page = (JSONObject) parser.parse(sb.toString());  
            jsonArrObj = (JSONObject) ((JSONArray) page.get("TfcacdarM")).get(1);  
  
            for(int j=0; j< ((JSONArray) jsonArrObj.get("row")).size();j++){  
                JSONObject row =(JSONObject) ((JSONArray) jsonArrObj.get("row")).get(j);    
                String sql = " INSERT INTO op"  
                        + " VALUES(?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)";  
                stmt = conn.prepareStatement(sql);//3. Query 준비  
                stmt.setString(1, (String) row.get("SIGUN_NM"));  
                stmt.setInt(2, Integer.parseInt((String) row.get("ACDNT_YY")));  
                stmt.setString(3, (String) row.get("ACDNT_DIV_NM"));  
                stmt.setLong(4, (Long) row.get("MULTI_KNOWLG_DIV_NO"));  
                stmt.setLong(5, (Long) row.get("MULTI_KNOWLG_DIV_GROUP_NO"));  
                stmt.setLong(6, (Long) row.get("LEGALDONG_CD_NO"));  
                stmt.setInt(7, Integer.parseInt((String) row.get("SPOT_NO")));  
                stmt.setString(8, (String) row.get("JURISD_POLCSTTN_NM"));  
                stmt.setString(9, (String) row.get("LOC_INFO"));  
                stmt.setLong(10, (Long) row.get("OCCUR_CNT"));  
                stmt.setLong(11,(Long)row.get("CASLT_CNT"));  
                stmt.setLong(12, (Long) row.get("DPRS_CNT"));  
                stmt.setLong(13, (Long)row.get("SERINJRY_INDVDL_CNT"));  
                stmt.setLong(14, (Long)row.get("SLTINJRY_INDVDL_CNT"));  
                stmt.setLong(15, (Long) row.get("INJURY_APLCNT_CNT"));  
                stmt.setDouble(16, Double.parseDouble((String) row.get("LAT")));  
                stmt.setDouble(17, Double.parseDouble((String) row.get("LOGT")));  
                stmt.setString(18, (String) row.get("MULTI_REGION_INFO"));  
                //4. Query 실행 및 리턴  
                int res = stmt.executeUpdate();  
                if(res > 0) {  
                    System.out.println("입력 성공");  
                }else {  
                    System.out.println("입력 실패");  
                }  
  
            }  
        }  
        dbConnection.disConnect(stmt,conn);  
  
  
    }  
  
  
}
```




---
참조 - https://hy-ung.tistory.com/56 open api 값 가져오기


https://m.blog.naver.com/myhouse34/222284326241