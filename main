import gspread
from oauth2client.service_account import ServiceAccountCredentials
import telegram
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext
from telegram import Update

# Set up Google Sheets API
scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
creds = ServiceAccountCredentials.from_json_keyfile_name('path/to/your/credentials.json', scope)
client = gspread.authorize(creds)
sheet = client.open('Your Google Sheet Name').sheet1

# Set up Telegram Bot
TOKEN = 'YOUR_TELEGRAM_BOT_TOKEN'
updater = Updater(token=TOKEN, use_context=True)

def get_answer(keyword):
    data = sheet.get_all_records()
    for row in data:
        if row['Keyword'] == keyword:
            return row['Answer']
    return "Sorry, I don't have an answer for that."

def reply(update: Update, context: CallbackContext):
    user_message = update.message.text
    answer = get_answer(user_message)
    context.bot.send_message(chat_id=update.effective_chat.id, text=answer)

def main():
    dp = updater.dispatcher
    dp.add_handler(MessageHandler(Filters.text & ~Filters.command, reply))
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()
