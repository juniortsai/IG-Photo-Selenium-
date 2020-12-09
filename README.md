# IG-Photo-Selenium-
運用Selenium 抓取IG Photo 
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait
import time
import os
import wget

# ChromeOptions = webdriver.ChromeOptions()
# ChromeOptions.add_argument('--headless')
# driver=webdriver.Chrome(options=ChromeOptions)
driver = webdriver.Chrome()
driver.get("http://www.instagram.com/")

username = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CSS_SELECTOR, "input[name='username']")))
password = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CSS_SELECTOR, "input[name='password']")))

username.send_keys("caic7559@gmail.com")
password.send_keys("test123456789")
button = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CSS_SELECTOR, "button[type='submit']"))).click()

#nadle 稍後再說

checkin = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.CLASS_NAME, "cmbtv"))).click()
checkin = WebDriverWait(driver, 5).until(EC.element_to_be_clickable((By.CLASS_NAME, "aOOlW.HoLwm "))).click()


#target the search input field
searchbox = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CLASS_NAME, "XTCLo.x3qfX")))

searchbox.clear()
#search for the hashtag cat
keyword = "#DOG"
searchbox.send_keys(keyword)
searchbox.send_keys(Keys.RETURN)
# Wait for 5 seconds
time.sleep(1)
searchbox.send_keys(Keys.ENTER)
time.sleep(1)
searchbox.send_keys(Keys.ENTER)
time.sleep(1)

#scroll down to scrape more images
driver.execute_script("window.scrollTo(0, 4000);")

#target all images on the page
images = driver.find_elements_by_tag_name('img')
images = [image.get_attribute('src') for image in images]
images = images[:-2]

print('Number of scraped images: ', len(images))
path = os.getcwd()
path = os.path.join(path, keyword[1:] + "s")

#create the directory
os.mkdir(path)
print(path)
#download images
counter = 0
for image in images:
    save_as = os.path.join(path, keyword[1:] + str(counter) + '.jpg')
    wget.download(image, save_as)
    counter += 1
driver.quit()

