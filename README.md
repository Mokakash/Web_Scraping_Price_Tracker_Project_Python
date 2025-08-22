# Web Scraping Price Tracker

This Python project originated from a personal need to track the price of the Apple Watch Ultra 2 on the Amazon U.S. website. Initially desiring to purchase the Apple Watch Ultra 2 without paying its full MSRP of $799, I’ve decided to build this project to monitor its price changes and receive email notifications if it dropped below the original price. 
The project aimed to assist in securing the watch at a discounted rate, bypassing the need to pay the full retail price. Through Python, this project streamlines the process of monitoring online prices, offering updates via email notifications.

### Workflow Overview
Below, you'll find a comprehensive overview detailing the essential stages of this Python project.

**Import the Necessary Libraries**

In the initial step, import essential Python libraries including:
+ `requests` for making HTTP requests.
+ `BeautifulSoup` for HTML parsing. 
+ `smtplib` for email-sending functionality.
+ `ssl` for secure connections. 

```
import requests
from bs4 import BeautifulSoup as bs
import smtplib
import ssl
```

**Define the URL and Create the User Agent Info**

The second step involves defining the target URL for the Apple Watch Ultra 2 on Amazon.com, enabling price monitoring for any alterations. Additionally, setting up a user agent string, mimicking genuine user behavior on Amazon's website plays a crucial role in avoiding server identification as a bot during the scraping process.

Note: 
+ Customize the 'URL' variable to match your desired product's webpage for accurate price tracking.
+ Adjust the 'User-Agent' to reflect your specific User Agent string (easily found by searching 'My User Agent' on Google).

```
URL = 'https://www.amazon.com/Apple-Cellular-Smartwatch-Precision-Extra-Long/dp/B0CHX7C4CL/ref=sr_1_5?crid=38JYNSLCGRVPU&keywords=apple%2Bwatch&qid=1704051072&sprefix=apple%2Bwatch%2Caps%2C116&sr=8-5&ufe=app_do%3Aamzn1.fos.ac2169a1-b668-44b9-8bd0-5ec63b24bcb5&th=1'

header = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36'}
```

**Create the Price Extractor Function**

In this step, I built a function to retrieve a webpage, parse its HTML content using `BeautifulSoup`, and specifically target the price within a designated span element identified by its ID (which can be located by inspecting the product page on Amazon). Additionally, I formatted and converted the extracted text into a float value, resulting in the function returning the accurate product price.

```
def extract_price():
    page = requests.get(URL, headers=header)
    soup = bs(page.content, 'html.parser')
    price = float(soup.find('span', id="a-price-whole").text.split()[0].replace(',',""))
    return price
```

**Create the Email Notifier Function**

A function has been crafted to automate email notifications, utilizing Gmail's SMTP server. It generates an alert if the Apple Watch Ultra 2's price falls below the set threshold of $779, prompting the recipient to inspect the provided Amazon URL for the price alteration. 

Note: Personal email credentials need to be entered for sender and receiver email addresses, as well as the app password.
+ `Sender_email = ‘SENDER EMAIL ADDRESS’`
+ `Receiver_email = ‘RECEIVER EMAIL ADDRESS’`
+ `Password = ‘YOUR APP PASSWORD’`
  + To get your app password either search for “My App Password” on Google or use this URL: *https://myaccount.google.com/apppasswords*

```
def send_email():
    port = 465
    smtp_server = "smtp.gmail.com"
    sender_email = "kashinejad85@gmail.com"
    receiver_email = "kashinejad85@gmail.com"
    password = 'xxxxxxxxxxxxxxxx'
    subject = 'Price Dropped for Apple Watch Ultra 2!!!'
    message = f"""
    The Apple Watch Ultra 2 price has dropped below $779 - check the Amazon link!
    Link = {URL}
               """
```

Utilizing the Email Library's `EmailMessage` class to configure the email's content, secure a connection using SMTP_SSL, authenticate the sender, and dispatch the email. This ensures secure transmission and sender verification during the email delivery process.

```
em = EmailMessage()
em['From'] = sender_email
em['To'] = receiver_email
em['Subject'] = subject
em.set_content(message)
context = ssl.create_default_context()
with stmplib.SMTP_SSL(smtp_server, port, context=context) as server:
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, em.as_string())
    print("Message Sent")
```

**Create Criteria and the Driver Code**

This section establishes the price criteria by setting a `my_price` threshold. The code snippet checks if the extracted price falls below this threshold using `extract_price()`. If the condition is met, the `send_email()` function is called, prompting an email notification to the `receiver_email` about the price drop.

Note: `my_price` threshold, is currently set at $799 for monitoring the Apple Watch Ultra 2. Please ensure to modify this number to your desired price for tracking other products.

```
my_price = 799
if extract_price() < my_price:
    send_email()
```

### Conclusion
This Python-based Price Tracker project is designed to monitor online product prices with a focus on the Apple Watch Ultra 2 as an initial reference. The project utilizes web scraping techniques to extract the product's price from a specified URL on Amazon.com. With a set threshold price, the system triggers an email notification to the user (me in this example) if the price drops below the defined threshold ($799).
Please feel free to modify the URL and adapt the code to track prices for your desired products.

### Contribution
Contributions to this project are welcome and encouraged. If you have ideas for enhancements or new features, feel free to fork the repository, make changes, and submit pull requests. Additionally, bug reports or suggestions for improvements are valuable contributions to the development of this price-tracking tool. Your input will help in enhancing the functionality and versatility of this Python-based project.
