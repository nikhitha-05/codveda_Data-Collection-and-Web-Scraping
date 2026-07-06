import requests
from bs4 import BeautifulSoup
import pandas as pd
import time

BASE_URL = "https://books.toscrape.com/catalogue/page-{}.html"

books = []

page = 1

print("Starting Web Scraping...\n")

while True:
    url = BASE_URL.format(page)

    try:
        response = requests.get(url, timeout=10)

        if response.status_code != 200:
            print("No more pages found.")
            break

        soup = BeautifulSoup(response.text, "html.parser")

        book_list = soup.find_all("article", class_="product_pod")

        if not book_list:
            break

        print(f"Scraping Page {page}...")

        for book in book_list:

            title = book.h3.a["title"]

            price = book.find("p", class_="price_color").text.strip()

            availability = (
                book.find("p", class_="instock availability")
                .text.strip()
            )

            rating = book.find("p")["class"][1]

            product_link = (
                "https://books.toscrape.com/catalogue/"
                + book.h3.a["href"].replace("../", "")
            )

            books.append({
                "Title": title,
                "Price": price,
                "Availability": availability,
                "Rating": rating,
                "Product Link": product_link
            })

        page += 1
        time.sleep(1)

    except Exception as e:
        print("Error:", e)
        break

# Create DataFrame
df = pd.DataFrame(books)

# Save CSV
df.to_csv("books.csv", index=False)

# Save JSON
df.to_json("books.json", orient="records", indent=4)

print("\n==============================")
print("Web Scraping Completed!")
print(f"Total Books Scraped: {len(df)}")
print("CSV File Saved: books.csv")
print("JSON File Saved: books.json")
print("==============================")

print("\nFirst 5 Records:\n")
print(df.head())
