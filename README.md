# ai-camera-lens-app
something im working on currently an ai camera app that identifies every product and gives a price, locatin, store to buy it from, or general  information, etc
import cv2

# Initialize camera
cap = cv2.VideoCapture(0)

# Capture image
ret, frame = cap.read()

# Release camera
cap.release()
import tensorflow as tf
from tensorflow import keras

# Load model
model = keras.models.load_model('product_classifier.h5')

# Preprocess image
img = cv2.resize(frame, (224, 224))
img = img / 255.0
img = img.reshape((1, 224, 224, 3))

# Predict product
prediction = model.predict(img)
import tensorflow as tf
from tensorflow import keras

# Load model
model = keras.models.load_model('product_classifier.h5')

# Preprocess image
img = cv2.resize(frame, (224, 224))
img = img / 255.0
img = img.reshape((1, 224, 224, 3))

# Predict product
prediction = model.predict(img)
import requests
from bs4 import BeautifulSoup

# Search for product
query = ' '.join(prediction)
url = f'https://www.google.com/search?q={query}'
response = requests.get(url)

# Scrape information
soup = BeautifulSoup(response.text, 'html.parser')
price = soup.find('div', {'class': 'BNeawe iBp4i AP7Wnd'}).get_text()
location = soup.find('div', {'class': 'BNeawe s3v9rd AP7Wnd'}).get_text()
store = soup.find('div', {'class': 'BNeawe s3v9rd AP7Wnd'}).find_next_sibling().get_text()
# Display information
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(frame, f'Price: {price}', (50, 50), font, 1, (255, 255, 255), 2, cv2.LINE_AA)
cv2.putText(frame, f'Location: {location}', (50, 100), font, 1, (255, 255, 255), 2, cv2.LINE_AA)
cv2.putText(frame, f'Store: {store}', (50, 150), font, 1, (255, 255, 255), 2, cv2.LINE_AA)

# Show image
cv2.imshow('Product Information', frame)
cv2.waitKey(0)
cv2.destroyAllWindows()
