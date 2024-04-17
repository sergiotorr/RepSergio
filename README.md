EXPRESION REGULAR IDENTIFICAR CURP

[A-Z]{4}\d{6}[H|M][A-Z]{5}(\d{2}|[A-Z]{1}\d{1})) esta expresion regular ayuda a saber si una CURP está bien escrita sea esta real o no o al menos siguiendo la serie de digitos de una CURP

Evidencia:
![image](https://github.com/sergiotorr/RepSergio/assets/160956566/a05030ae-226c-482a-95b3-e23a620cb2c3)


Codigo:


import logging
import re

from telegram import ForceReply, Update
from telegram.ext import Application, CommandHandler, ContextTypes, MessageHandler, filters

logging.basicConfig(format="%(asctime)s - %(name)s - %(levelname)s - %(message)s", level=logging.INFO)
logging.getLogger("httpx").setLevel(logging.WARNING)
logger = logging.getLogger(__name__)

expresion_regular = re.compile(r"([A-Z]{4}\d{6}[H|M][A-Z]{5}(\d{2}|[A-Z]{1}\d{1}))", re.IGNORECASE)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Send a message when the command /start is issued."""
    user = update.effective_user
    await update.message.reply_html(
        rf"Ingresa tu CURP {user.mention_html()}.",
        reply_markup=ForceReply(selective=True),
    )


async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Send a message when the command /help is issued."""
    await update.message.reply_text("Help!")


async def echo(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    """Echo the user message if it matches the regular expression."""
    message_text = update.message.text
    if expresion_regular.search(message_text):
        await update.message.reply_text("Tu CURP es valida")
    else:
        await update.message.reply_text("Tu CURP no es valida")

def main() -> None:
    """Start the bot."""
    application = Application.builder().token("7020724987:AAEILDNGFsZcD41yB0ptf1k9GFI3WN9uM54").build()

    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("help", help_command))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, echo))

    application.run_polling(allowed_updates=Update.ALL_TYPES)


if __name__ == "__main__":
    main()
