# ECPay HTTP Request Analysis

## Header

- ### Host: `vendor.ecpay.com.tw`
- ### User-Agent: `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:71.0) Gecko/20100101 Firefox/71.0`
- ### Cookies
  - ASP.NET_SessionId
  - ecpay_allpay_ecpay
  - ecpay_allpay_ecpay_ext
  - ecpay_vendor_allpay_ecpay
  - ecpay_vendor_allpay_ecpay_ext
  - gw_allpay_p
  - vendor_allpay_p

## Payload

- ### Login
  - type: POST
  - url: `/User/LogOn_Step2`
  - arguments:

    | Name | Info | Value | Condition |
    | ---- | ---- | ----- | --------- |
    | *Account | 帳號 |
    | *AuthID | | 1 | *CONSTANT* |
    | *grecaptchaResponse | | *None* | *CONSTANT* |
    | *AuthNO | 身分證末四碼 |
    | *Password | 密碼 |
    | *CaptchaValue | 圖形驗證碼 |
    | *_mvcCaptchaGuid | 圖形GUID |

- ### Login Finish
  - type: POST
  - url: `/User/LogOnFinish`
  - arguments:

    | Name | Info | Value | Condition |
    | ---- | ---- | ----- | --------- |
    | FinishedRtnCode | | 1 | *CONSTANT* |

- ### Query AIO Trade
  - type: POST
  - url: `/TradeNoAio/Query`
  - arguments:

    | Name | Info | Value | Condition |
    | ---- | ---- | ----- | --------- |
    | QueryDate | | 6: 訂單日期 (Default)<br>4: 撥款日期<br>2: 付款日期 |
    | StartDate | 開始日期 | YYYY-MM-DD |
    | EndDate | 結束日期 | YYYY-MM-DD |
    | PaymentStatus | 付款狀態 | *None*: 全部 (Default)<br>0: 未付款<br>1: 已付款<br>2: 訂單失敗 |
    | PaymentTypeValue | 付款分類 | 0: 全部 (Default)<br>1: 信用卡<br>2: 非信用卡 |
    | PaymentTypeID | 付款方式 | *None*: 全部 (Default)<br>10000: 信用卡<br>10001: 網路ATM<br>10002: ATM 櫃員機<br>10003: 超商代碼<br>10004: 超商條碼<br>10043: 超商手機條碼 |
    | CreditPaymentType | 信用卡付款方式 | *None*: 全部 (Default)<br>1: 一次付清<br>2: 分期<br>3: 定期定額 |
    | OptQueryTradeNo | | OptMerchantTradeNo: 廠商訂單編號 (Default)<br>OptAllPayTradeNo: 綠界金流訂單編號 |
    | MerchantTradeNo | 廠商訂單編號 | | OptQueryTradeNo = OptMerchantTradeNo |
    | AllPayTradeNo | 綠界金流訂單編號 | | OptQueryTradeNo = OptAllPayTradeNo |
    | ChildMerchantID | 商店代號 |
    | gwsr | 信用卡授權單號 |
    | card4no | 信用卡卡號末4碼 |
    | AllocateStatus | 撥款狀態 | *None*: 全部 (Default)<br>0: 未撥款<br>1: 已撥款 |
    | vAccount | ATM虛擬帳號 |
    | PaymentNo | 超商代碼 |
    | StoreID | 店鋪代號 |
    | PageSize | 每頁顯示筆數 | 10 (Default)<br>20<br>50<br>100 |
    | PageSize2 | | 10 | *CONSTANT* |

- ### Query Pay Link
  - type: POST
  - url: `/QuickCollect/QueryPayLink`
  - arguments:

    | Name | Info | Value | Condition |
    | ---- | ---- | ----- | --------- |
    | *StartDate | 開始日期 | YYYY-MM-DD |
    | *EndDate | 結束日期 | YYYY-MM-DD |
    | URLStatus | 連結狀態 | 1: 開啟 (Default)<br>2: 關閉<br>4: 違規下架 |
    | QueryAll | 所有開啟連結 | 0: 根據其他參數篩選<br>1: 所有開啟連結 |

- ### Create Pay Link
  - type: POST
  - url:
  - arguments:

    | Name | Info | Value | Condition |
    | ---- | ---- | ----- | --------- |
    | *QuickPay.QuickPayName | 收款連結名稱 |
    | QuickPay.TradeType | | 1 | *CONSTANT* |
    | TranType | 交易方式 | on: 單筆收款<br>off: 定期定額收款 |
    | commodityType | | on: 固定商品名稱<br>off: 由消費者填寫 |
    | *QuickPay.ProductName | 商品名稱 |
    | tradeAMTType | 金額形式 | on: 固定金額<br>off: 由消費者填寫 |
    | QuickPay.TradeAMT | 金額 | | tradeAMTType = on |
    | chkPaymentType | 收款方式 | *None* |
    | QuickPay.CreditChoose.Installments | | *None* | *CONSTANT* |
    | QuickPay.ATMPayExpire | 繳費有效期限 | 1~30 |
    | QuickPay.CreditChoose.PeriodicSettings.Freq | 扣款頻率 | 1~12 | tradeAMTType = off |
    | QuickPay.CreditChoose.PeriodicSettings.Count | 限制扣款次數 | 2~99 | tradeAMTType = off |
    | QuickPay.CreditChoose.Type | OneLumpSum <br>Periodic | |TranType = on<br>TranType = off  |
    | QuickPay.CreditChoose.PeriodicSettings.Cycle | 扣款頻率單位 | month | TranType = off *CONSTANT* |
    | QuickPay.PaymentType | 收款方式 | 參數以`#`連接 |
    | PeriodStatus | 有效時間 | PeriodOn: 設定有效時間<br>PeriodOff: 永遠有效 |
    | QuickPay.StartDate | 開始有效時間 | YYYY-MM-DD hh:mm | PeriodStatus = PeriodOn |
    | QuickPay.EndDate | 結束有效時間 | YYYY-MM-DD hh:mm | PeriodStatus = PeriodOn |
    | QuickPay.AddAddressStatus | 寄送地址 | 1: 付款人需填寫寄送地址<br>2: 付款人不需填寫寄送地址 |
    | QuickPay.PreRemark | 備註預填文字 |
    | QuickPay.ClientBackURL | 返回商店按鈕 |
