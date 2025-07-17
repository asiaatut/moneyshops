**import logging
from aiogram import Bot, Dispatcher, executor, types

API_TOKEN = 'ВАШ_ТОКЕН_ТЕЛЕГРАМ_БОТА'

logging.basicConfig(level=logging.INFO)
bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)

PRODUCT_NAME = "Чай Кратом"
PRICES = {
    "50г": 800,
    "100г": 1500
}

@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    await message.reply(
        f"Добро пожаловать в магазин автопродаж!\n"
        f"У нас в наличии:\n\n"
        f"{PRODUCT_NAME}\n"
        f"1. 50 г — {PRICES['50г']} грн\n"
        f"2. 100 г — {PRICES['100г']} грн\n\n"
        f"Для заказа напишите, какой объем вы хотите приобрести (50 или 100 г)."
    )

@dp.message_handler(lambda message: message.text)
async def handle_order(message: types.Message):
    text = message.text.strip().lower()
    if "50" in text:
        await message.reply(
            f"Вы выбрали {PRODUCT_NAME} 50 г за {PRICES['50г']} грн.\n"
            "Пожалуйста, отправьте ваш номер телефона для оформления заказа."
        )
    elif "100" in text:
        await message.reply(
            f"Вы выбрали {PRODUCT_NAME} 100 г за {PRICES['100г']} грн.\n"
            "Пожалуйста, отправьте ваш номер телефона для оформления заказа."
        )
    elif text.replace("+", "").replace("-", "").isdigit() and len(text) >= 10:
        await message.reply(
            "Спасибо за заказ! Наш менеджер свяжется с вами для уточнения деталей."
        )
    else:
        await message.reply(
            "Пожалуйста, напишите '50' или '100', чтобы выбрать объем, или отправьте номер телефона для оформления заказа."
        )

if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
    **
