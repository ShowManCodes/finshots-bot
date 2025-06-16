from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
import requests
from bs4 import BeautifulSoup

TOKEN = "7949745130:AAEqXYfXUcFASFzUphjnsYHEAtYzKL-qqck"

# 🟩 Function to fetch the latest Finshots article
import feedparser

def get_finshot():
    feed_url = "https://finshots.in/archive/rss/"
    feed = feedparser.parse(feed_url)

    if feed.entries:
        latest = feed.entries[0]
        title = latest.title
        summary = latest.summary
        link = latest.link
        return f"📰 *{title}*\n\n{summary}\n\n[Read more]({link})"
    else:
        return "❌ Could not fetch the article from RSS feed."




# 🟩 Bot command: /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Welcome! Type /finshot to get today’s Finshots article 🗞️")

# 🟩 Bot command: /finshot
async def finshot(update: Update, context: ContextTypes.DEFAULT_TYPE):
    article = get_finshot()
    await update.message.reply_markdown(article)

# 🟩 Start the bot
if __name__ == '__main__':
    app = ApplicationBuilder().token(TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("finshot", finshot))

    print("Bot is running...")
    app.run_polling()

