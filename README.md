# webtables - Automation
## Harish kumar P 
## 212223060086

### Code:

```

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

driver = webdriver.Chrome()
driver.get("https://assertqa.com/practice/webtables")

wait = WebDriverWait(driver, 10)
wait.until(EC.presence_of_element_located((By.TAG_NAME, "table")))

headers = driver.find_elements(By.XPATH, "//table//th")
headings = [h.text.strip() for h in headers]

print("\nTC01 - Table Headings:", headings)

def get_rows():
    return driver.find_elements(By.XPATH, "//table//tbody/tr")

rows = get_rows()
print("\nTC02 - First Row:", rows[0].text if rows else "No data")

data = []
while True:
    for row in get_rows():
        if row.text.strip() and row.text.strip() not in data:
            data.append(row.text.strip())

    nxt = driver.find_elements(By.XPATH, "//button[contains(.,'Next')]")
    if not nxt or not nxt[0].is_enabled():
        break
    nxt[0].click()
    time.sleep(1)

print("\nTC03 - Last Row:", data[-1] if data else "No data")

search = driver.find_element(By.XPATH, "//input[contains(@placeholder,'Search')]")
search.send_keys("Smith")
time.sleep(1)

found = [r.text for r in get_rows() if "Smith" in r.text]
print("\nTC04 - Search Result:", found if found else "No match")
search.clear()

emails = []
while True:
    for row in get_rows():
        for cell in row.find_elements(By.TAG_NAME, "td"):
            value = cell.text.strip()
            if "@" in value and value not in emails:
                emails.append(value)

    nxt = driver.find_elements(By.XPATH, "//button[contains(.,'Next')]")
    if not nxt or not nxt[0].is_enabled():
        break
    nxt[0].click()
    time.sleep(1)

print("\nTC05 - Emails:", emails)

print("\nTC06 - Due Column:", "Due" in headings)

links = driver.find_elements(By.XPATH, "//table//a")
print("\nTC07 - Website Links:", [l.get_attribute("href") for l in links] if links else "No links found")

print("\nTC08 - Total Records:", len(data))

driver.quit()

```
