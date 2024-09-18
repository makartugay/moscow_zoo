from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes, CallbackQueryHandler

# Переменная с информацией о животных
TOTEM_ANIMALS = {
    "медведь": "Медведь символизирует силу, уверенность и стойкость. В Московском зоопарке можно увидеть бурого медведя.",
    "волк": "Волк символизирует свободу и независимость. В Московском зоопарке содержатся серые волки.",
    "сова": "Сова является символом мудрости и знаний. В зоопарке можно увидеть различные виды сов.",
    "тигр": "Тигр символизирует мужество и решительность. В Московском зоопарке живут амурские тигры."
}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    keyboard = [
        [InlineKeyboardButton("Медведь", callback_data='медведь'),
         InlineKeyboardButton("Волк", callback_data='волк')],
        [InlineKeyboardButton("Сова", callback_data='сова'),
         InlineKeyboardButton("Тигр", callback_data='тигр')]
    ]

    reply_markup = InlineKeyboardMarkup(keyboard)

    await update.message.reply_text('Привет! Я бот Московского зоопарка. Выберите животное для получения информации:', reply_markup=reply_markup)

async def handle_button_click(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    query = update.callback_query
    await query.answer()  # Обязательно вызываем ответ на нажатие кнопки

    animal = query.data
    response = TOTEM_ANIMALS.get(animal, "Извините, информация о данном животном отсутствует. Попробуйте другое животное.")

    await query.edit_message_text(text=response)

def main():
    TOKEN = '7022857202:AAH_L_jaR0wiad59PZcAskRG0uXTMUrzPaI'

    application = Application.builder().token(TOKEN).build()

    application.add_handler(CommandHandler('start', start))
    application.add_handler(CallbackQueryHandler(handle_button_click))  # Добавляем обработчик нажатий на кнопки

    application.run_polling()

if __name__ == '__main__':
    main()
