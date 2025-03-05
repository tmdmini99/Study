

```java
switch (requestUri) {  
            case "/ledger/2102CUD":  
                paramVo.setAlarmType("001");  
                paramVo.setMessage(paramVo.getPk_LEDGER_ID() + " 원장이 발급되었습니다.");  
                paramVo.setMessageEn("Ledger number " + paramVo.getPk_LEDGER_ID() + " has been issued.");  
                break;  
  
            case "/ledger/2100CUD":  
                paramVo.setAlarmType("002");  
                paramVo.setMessage(paramVo.getPk_LEDGER_ID() + " 원장에 " + paramVo + "원 결제되었습니다.");  
                paramVo.setMessageEn("An amount of " + paramVo.getPk_LEDGER_ID() + " KRW has been paid for ledger number " + paramVo.getPk_LEDGER_ID() + ".");  
                break;  
  
            case "/ledger/2101CUD":  
                paramVo.setAlarmType("003");  
                paramVo.setMessage(paramVo.getPk_LEDGER_ID() + " 원장에 " + paramVo.getProductNm() + "이 " + paramVo.getStatus() + " 처리 되었습니다.");  
                paramVo.setMessageEn("The product " + paramVo.getProductNm() + " in ledger number " + paramVo.getPk_LEDGER_ID() + " has been processed with status " + paramVo.getStatus() + ".");  
                break;  
  
            case "/refund/1500CUD":  
                paramVo.setAlarmType("004");  
                paramVo.setMessage(paramVo.getRefundId() + " 환불내역에 " + paramVo.getProductNm() + " 상품이 " + paramVo.getQuantity() + "개 " + paramVo.getStatus() + " 처리 되었습니다.");  
                paramVo.setMessageEn("The product " + paramVo.getProductNm() + " in refund record " + paramVo.getRefundId() + " has been processed with status " + paramVo.getStatus() + ", quantity: " + paramVo.getQuantity() + ".");  
                break;  
//          수출 송장은 아직  
//            case "/invoice/005CUD":  
//                paramVo.setAlarmType("005");  
//                paramVo.setMessage(paramVo.getInvoiceId() + " 수출송장이 발급되었습니다.");  
//                paramVo.setMessageEn("Export invoice number " + paramVo.getInvoiceId() + " has been issued.");  
//                break;  
//  
//            case "/invoice/006CUD":  
//                paramVo.setAlarmType("006");  
//                paramVo.setMessage(paramVo.getInvoiceId() + " 수출송장에 대해 입력이 필요합니다.");  
//                paramVo.setMessageEn("Input is required for export invoice number " + paramVo.getInvoiceId() + ".");  
//                break;  
  
            case "/user/101CUD":  
                paramVo.setAlarmType("101");  
                paramVo.setMessage(paramVo.getUserId() + " 계정의 가입신청이 도착했습니다.");  
                paramVo.setMessageEn("A registration request has arrived for the account " + paramVo.getUserId() + ".");  
                break;  
  
            case "/order/1300CUD":  
                paramVo.setAlarmType("102");  
                paramVo.setMessage(paramVo.getUserId() + " 사용자가 " + paramVo.getOrderId() + " 주문을 등록했습니다.");  
                paramVo.setMessageEn(paramVo.getUserId() + " has placed order " + paramVo.getOrderId() + ".");  
                break;  
  
            case "/order/1400CUD":  
                paramVo.setAlarmType("103");  
                paramVo.setMessage(paramVo.getUserId() + " 사용자가 " + paramVo.getOrderId() + " 주문을 취소했습니다.");  
                paramVo.setMessageEn(paramVo.getUserId() + " has canceled order " + paramVo.getOrderId() + ".");  
                break;  
  
            case "/order/1400CUD":  
                paramVo.setAlarmType("104");  
                paramVo.setMessage(paramVo.getUserId() + " 사용자가 " + paramVo.getOrderId() + " 주문의 " + paramVo.getProductNm() + " 주문을 취소했습니다.");  
                paramVo.setMessageEn(paramVo.getUserId() + " has canceled the item " + paramVo.getProductNm() + " in order number " + paramVo.getOrderId() + ".");  
                break;  
  
            case "/order/1500CUD":  
                paramVo.setAlarmType("105");  
                paramVo.setMessage(paramVo.getUserId() + " 사용자가 " + paramVo.getRefundId() + " 환불을 신청했습니다.");  
                paramVo.setMessageEn(paramVo.getUserId() + " has requested a refund for refund number " + paramVo.getRefundId() + ".");  
                break;  
  
//            case "/invoice/2200CUD":  
//                paramVo.setAlarmType("106");  
//                paramVo.setMessage(paramVo.getInvoiceId() + " 수출송장 사용자 입력이 완료되었습니다.");  
//                paramVo.setMessageEn("User input for export invoice number " + paramVo.getInvoiceId() + " has been completed.");  
//                break;  
  
            default:  
                paramVo.setAlarmType("000");  
                paramVo.setMessage("알 수 없는 요청입니다.");  
                paramVo.setMessageEn("Unknown request.");  
                break;  
        }
```