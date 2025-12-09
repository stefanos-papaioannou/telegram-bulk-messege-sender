# Telegram User Account Sender Documentation

This project provides a Python script to send messages from a Telegram user account to phone numbers, supporting both single message sending and bulk sending from an Excel file.

## Features

-   **User Account Integration**: Connects to Telegram using a user's API ID, API Hash, and phone number.
-   **Single Message Send**: Send a message to a specific phone number.
-   **Smart Contact Handling**: Attempts to send messages directly, and if that fails, tries to import the contact and then send, or resolve the user.
-   **Bulk Sending from Excel**: Reads phone numbers from an Excel file and sends a predefined message to all valid numbers with configurable delays.
-   **Rate Limit Handling**: Includes basic rate-limiting awareness with configurable delays between messages to avoid flood waits.

-   ## Setup

### 1. Install Dependencies

Run the first cell in the notebook to install all necessary libraries:

```python
!pip install pyrogram tgcrypto pandas openpyxl -q
```

### 2. Configure API Credentials

Update `CELL 2` with your Telegram API credentials. You can obtain these from [my.telegram.org](https://my.telegram.org/). Make sure to use your **user account API** credentials, not a bot token.

-   `API_ID`: Your API ID.
-   `API_HASH`: Your API Hash.
-   `PHONE_NUMBER`: Your Telegram phone number (e.g., `+3069XXXXXXXX`).

```python
API_ID = # Replace with your API ID
API_HASH = "" # Replace with your API Hash
PHONE_NUMBER = "" # Replace with your phone number
```

**Important**: If `API_HASH` is still `"your_api_hash_here"`, the script will prompt you to update it.

## Usage

### 1. Test User Account Connection

Run `CELL 4` (`test_user_connection()`) to verify that your API credentials are correct and that the script can successfully connect to your Telegram account.

```python
await test_user_connection()
```

### 2. Send a Single Test Message

To send a single message to a specific phone number, run `CELL 6` (`single_test()`). You will be prompted to enter a test phone number.

```python
await single_test()
```

### 3. Bulk Send Messages from Excel

To send messages to multiple phone numbers from an Excel file, run `CELL 5` (`bulk_send_excel()`).

**Expected Excel Format**:

The Excel file should contain a column with phone numbers. The column name will be specified during execution.

**Steps:**

1.  Run the `bulk_send_excel()` function.
2.  You will be prompted to upload your Excel file.
3.  After uploading, the script will display the columns found in your Excel file. You will then be asked to enter the exact name of the column containing the phone numbers.
4.  You will be prompted to enter the message you wish to send. A default message is provided if you leave it blank.
5.  A summary of the operation will be displayed, and you will be asked for confirmation before sending the messages.
6.  The script will then proceed with sending messages, showing progress and detailed results.

```python
await bulk_send_excel()
```
## Error Handling & Notes

-   **Rate Limits (`FLOOD_WAIT`)**: Telegram imposes rate limits. The `bulk_send_simple` function includes a delay between messages to mitigate this. If you encounter `FLOOD_WAIT` errors, consider increasing the `delay` parameter in the `bulk_send_simple` method.
-   **Privacy Settings**: Some users may have privacy settings that prevent direct messages from non-contacts. The script attempts workarounds (contact import), but some messages may still fail due to user privacy settings.
-   **Invalid Phone Numbers**: The script attempts to clean phone numbers (e.g., adding `+30` for Greek numbers), but ensures your Excel data is as clean as possible.
-   **Session Management**: The script starts and stops the Pyrogram session for each operation (`start_session()` and `stop_session()`). This ensures resources are managed correctly.
