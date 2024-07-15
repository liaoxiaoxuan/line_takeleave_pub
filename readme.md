# Line Take Leave Bot   

這個專案是透過 Line Bot 和 Line Notify，自動回覆家長請假的訊息，並將訊息轉傳至私人帳號或群組。  
此專案的動機在於自動化處理班級學生請假訊息，方便導師掌握學生出缺情況。  

## 專案簡介 

我本身是一位高職導師，每天需要掌握班上學生的出缺狀況。  
透過 LINE 為孩子請假，已是現今趨勢，而我所回覆的內容通常大同小異，不外乎讓家長知道導師已收到訊息、告知家長請假手。    
因此，開發了一個自動化系統來回覆這些請假訊息。  
此外，為了掌握班上學生的出缺狀況，系統會將家長的請假訊息轉傳 至我的私人帳號或群組。

## 功能 

1. 自動回覆家長幫孩子請假的 Line 訊息。 
2. 根據不同的請假類型提供相應的回覆內容。   
3. 將家長的請假訊息轉傳至私人帳號或群組。   

## 安裝說明 
1. 複製此專案到本地端   

    ```sh
    git clone https://github.com/liaoxiaoxuan/line_takeleave_pub.git
    ```

2. 進入專案目錄 

    ```sh
    cd line_takeleave_pub
    ```

3. 安裝所需套件   

    ```sh
    pip install -r requirements.txt
    ```

4. 配置設定檔案  
設置配置文件`config.json`，包含 LINE Bot 的 Channel Access Token、Channel Secret 和 Line Notify Token：
    ```json
    {
    "channel_access_token": "YOUR_CHANNEL_ACCESS_TOKEN",
    "channel_secret": "YOUR_CHANNEL_SECRET",
    "line_notify_token": "YOUR_LINE_NOTIFY_TOKEN"
    }
    ```

## 使用方法 

### 1. 啟動 Flask 應用    

```sh
python app.py
```

### 2. 設置 Webhook URL 

2-1. 登錄 LINE Developers   
2-2. 將 Webhook URL 設置為 `https://your-domain/callback`    

## 程式說明    

### 程式架構    
這個專案使用 Flask 框架來建立 Web 應用，並與 LINE Bot SDK 和 LINE Notify 進行互動。主要分為以下幾個模組：   

1. Flask 應用（app.py） 

- 主要負責接收來自 LINE 平台的 Webhook 請求，並根據收到的事件進行處理。  
- 包含了 `callback()` 函數來處理 `/callback` 的 POST 請求，並使用 `WebhookHandler` 驗證簽名及處理請求。    

2. LINE Bot SDK（linebot.v3）  

- 提供了 `WebhookHandler` 來處理 LINE 平台發送的事件，例如消息事件。   
- 使用 `MessagingApi` 來與 LINE Messaging API 進行互動，例如發送回覆消息。 

3. LINE Notify 

- 使用 `requests` 模組發送 HTTP POST 請求到 LINE Notify API，將收到的訊息轉發至指定的 LINE 帳號或群組。   

4. 設定檔案（config.json）  

- 包含了 channel_access_token、channel_secret 和 line_notify_token 的配置資訊，用於授權 LINE Bot 和 LINE Notify 的使用權限。    

### 主要函數   
- `callback()`：處理來自 `/callback` 的 POST 請求，驗證簽名並使用 WebhookHandler 處理請求。 
- `handle_message(event)`：處理 LINE 發送的訊息事件，根據訊息內容回覆相應的文字。   
- `replyMessage(user_message_lower)`：根據家長的請假訊息，自動回覆對應的內容。  
- `line_notify_image(message)`：透過 LINE Notify 將訊息轉傳至私人帳號或群組。   

### 主要模組    
- `Flask`：用於建立 Web 應用。    
- `linebot`：用於處理 LINE Bot 和 LINE Notify 的互動。    
- `requests`：用於發送 HTTP 請求。    

