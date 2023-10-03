import logging
from telegram import ReplyKeyboardMarkup, ReplyKeyboardRemove
from telegram.ext import Updater, CommandHandler, ConversationHandler, MessageHandler, Filters
from telegram import InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Updater, CallbackQueryHandler
 
TOKEN = "6562108160:AAFR4jYMtXJ4kWT6fEIJBjelmly160R1g7g"
updater = Updater(token=TOKEN, use_context=True)
dp = updater.dispatcher
 
CHARACTER_SELECTION, CHARACTER_STATS = range(2)
 
characters = {
    "Nobita": {
        "name": "Nobita Nobi",
        "gender": "Male",
        "age": "Unknown",
        "classification": "Human",
        "powers": ["Weapon Mastery", "Healing"],
        "speed": "17",
        "lifting_strength": "10",
        "striking_strength": "8",
        "durability": "17",
        "stamina": "13",
        "range": "Standard Melee Range physically",
        "intelligence": "Above Average",
        "weaknesses": ["Notable Attacks/Techniques"],
        "image": "https://telegra.ph/file/f15bd8e4a9f40e1dc2ff4.jpg"
    },
    "Gian": {
        "name": "Takeshi Gouda (Gian)",
        "gender": "Male",
        "age": "10-11",
        "classification": "Human",
        "powers": ["Sound Manipulation"],
        "speed": "16",
        "lifting_strength": "25",
        "striking_strength": "22",
        "durability": "20",
        "stamina": "25",
        "range": "Standard Melee",
        "intelligence": "Above Average",
        "weaknesses": ["Normal Human Weakness"],
        "image": "https://telegra.ph/file/96814dc9204b61916ba3b.jpg"
    },
    "Suneo": {
        "name": "Suneo Honekawa",
        "gender": "Male",
        "age": "10",
        "classification": "Human",
        "powers": ["Science And Technology", "Creation"],
        "speed": "25",
        "lifting_strength": "8",
        "striking_strength": "13",
        "durability": "15",
        "stamina": "18",
        "range": "Standard Melee",
        "intelligence": "High Intelligence",
        "weaknesses": ["Normal Human Weakness"],
        "image": "https://telegra.ph/file/f63db3f5808468d66c298.jpg"
    }
}
 
user_characters = {}
 
def start(update, context):
    user_id = update.message.from_user.id
    if user_id in user_characters:
        update.message.reply_text("You have started your journey before.")
        return -1
    else:
        keyboard = [[InlineKeyboardButton("Nobita", callback_data='Nobita'),InlineKeyboardButton("Gian", callback_data='Gian'),InlineKeyboardButton("Suneo", callback_data='Suneo')]]
        reply_markup = InlineKeyboardMarkup(keyboard)
 
        update.message.reply_text(f"Hello, {update.message.from_user.username}! Welcome to the Doraemon RPG Based Game Bot. Choose a character via the buttons below:", reply_markup=reply_markup)
        return CHARACTER_SELECTION
 
def character_selection(update, context):
    user_id = update.message.from_user.id
    query = update.callback_query
    character = query.data
    text = ''
 
    keyboard = [[InlineKeyboardButton("Nobita", callback_data='Nobita'),InlineKeyboardButton("Gian", callback_data='Gian'),InlineKeyboardButton("Suneo", callback_data='Suneo')]]
 
    reply_markup = InlineKeyboardMarkup(keyboard)
 
    if character in characters:
        char_stats = characters[character]
 
        text+= f"Character Stats for {char_stats['name']}\n\n"
        text+=f"Gender: {char_stats['gender']}\n"
        text+=f"Age: {char_stats['age']}\n"
        text+=f"Classification: {char_stats['classification']}\n\n"
        text+=f"Powers and Abilities: {', '.join(char_stats['powers'])}\n\n"
        text+=f"Speed: {char_stats['speed']}\n"
        text+=f"Lifting Strength: {char_stats['lifting_strength']}\n"
        text+=f"Striking Strength: {char_stats['striking_strength']}\n"
        text+=f"Durability: {char_stats['durability']}\n"
        text+=f"Stamina: {char_stats['stamina']}\n"
        text+=f"Range: {char_stats['range']}\n\n"
        text+=f"Intelligence: {char_stats['intelligence']}\n\n"
        text+=f"Weaknesses: {', '.join(char_stats['weaknesses'])}\n\n"
        text+=f"NOTE: THIS CHARACTER CAN'T BE USED IN ADVENTURES."
 
        context.bot.send_photo(update.effective_chat.id, photo = char_stats['image'], caption="Character Image" , reply_markup = reply_markup)
 
 
dp.add_handler(CommandHandler('start', start))
dp.add_handler(CallbackQueryHandler(character_selection, pattern="Nobita"))
dp.add_handler(CallbackQueryHandler(character_selection, pattern="Gian"))
dp.add_handler(CallbackQueryHandler(character_selection, pattern="Suneo"))
 
 
updater.start_polling()
updater.idle()
