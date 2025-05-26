import requests
from typing import Optional


def fetch_product_data(article: int) -> Optional[dict]:
    """
    Отправляет GET-запрос к Wildberries API и получает данные по товару.

    :param article: Артикул товара
    :return: Словарь со всеми данными о товаре, либо None
    """
    url = (
        "https://card.wb.ru/cards/v2/detail"
        "?appType=1&curr=rub&dest=-3902910&hide_dtype=13"
        f"&spp=30&ab_testing=false&lang=ru&nm={article}"
    )
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        data = response.json()
        products = data.get("data", {}).get("products", [])
        return products[0] if products else None
    except Exception as e:
        print(f"[fetch_product_data] Ошибка: {e}")
        return None


def parse_product_info(product: dict) -> Optional[dict]:
    """
    Извлекает название и цену товара из данных, полученных от Wildberries API.

    :param product: Словарь с данными о товаре
    :return: Словарь с полями 'name' и 'price', либо None
    """
    name = product.get("name")
    sizes = product.get("sizes", [])
    if not name or not sizes:
        return None

    price_info = sizes[0].get("price")
    if not price_info or "product" not in price_info:
        return None

    return {
        "name": name,
        "price": price_info["product"] // 100
    }


def get_wb_info(article: int) -> Optional[dict]:
    """
    Возвращает информацию о товаре Wildberries по его артикулу.

    :param article: Артикул товара
    :return: Словарь с названием и ценой, либо None, если товар не найден
    """
    product = fetch_product_data(article)
    if not product:
        return None
    return parse_product_info(product)


if __name__ == "__main__":
    result = get_wb_info(56565656) # 56565656 артикул для примера
    if result:
        print(f"Название: {result['name']}, Цена: {result['price']} рублей")
    else:
        print("Товар не найден")

