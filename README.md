# Automated-Python-emails
To automate sending daily email reports in Python, you can use the `smtplib` library to send emails and the `schedule` library to manage the daily task scheduling. Here's a step-by-step guide to setting it up:

### Step 1: Install Necessary Libraries
First, you need to install the necessary libraries. You can install `schedule` and `smtplib` (which is part of the Python standard library) using pip:

```bash
pip install schedule
```

### Step 2: Set Up Your Email Account
You'll need to use an email account to send the reports. For this example, we'll use Gmail. Ensure you enable "Less secure app access" in your Gmail account settings, or generate an app password if you have 2FA enabled.

### Step 3: Write the Python Script
Here's the complete script to send daily email reports:

```python
import smtplib
import schedule
import time
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

# Email credentials and recipient
EMAIL_ADDRESS = 'your_email@gmail.com'
EMAIL_PASSWORD = 'your_password'
RECIPIENT_EMAIL = 'recipient_email@example.com'

# Function to send email
def send_email():
    # Create message container
    msg = MIMEMultipart()
    msg['From'] = EMAIL_ADDRESS
    msg['To'] = RECIPIENT_EMAIL
    msg['Subject'] = 'Daily Report'

    # Create the body of the message (a plain-text and an HTML version)
    text = "This is your daily report."
    html = """\
    <html>
      <head></head>
      <body>
        <p>This is your daily report.<br>
           Have a great day!</p>
      </body>
    </html>
    """

    # Record the MIME types of both parts - text/plain and text/html.
    part1 = MIMEText(text, 'plain')
    part2 = MIMEText(html, 'html')

    # Attach parts into message container.
    msg.attach(part1)
    msg.attach(part2)

    # Send the message via local SMTP server.
    try:
        server = smtplib.SMTP('smtp.gmail.com', 587)
        server.starttls()
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        server.sendmail(EMAIL_ADDRESS, RECIPIENT_EMAIL, msg.as_string())
        server.quit()
        print("Email sent successfully")
    except Exception as e:
        print(f"Failed to send email: {e}")

# Schedule the email to be sent daily
schedule.every().day.at("08:00").do(send_email)

# Keep the script running
while True:
    schedule.run_pending()
    time.sleep(1)
```

### Step 4: Configure and Run the Script
1. **Modify the script with your credentials**: Replace `your_email@gmail.com`, `your_password`, and `recipient_email@example.com` with your actual email credentials and the recipient's email address.

2. **Set the report content**: Customize the `text` and `html` variables in the `send_email` function to include the content of your daily report.

3. **Set the schedule**: The line `schedule.every().day.at("08:00").do(send_email)` schedules the email to be sent daily at 8 AM. Adjust the time as needed.

4. **Run the script**: Execute the script by running:

```bash
python send_daily_report.py
```

### Step 5: Keep the Script Running
To ensure the script runs continuously, you might want to run it on a server or use a task scheduler like cron (on Unix-based systems) or Task Scheduler (on Windows). This ensures that the script starts automatically and continues running without manual intervention.

For example, on a Unix-based system, you can use `cron` to run the script at startup:

1. Open the crontab file:
    ```bash
    crontab -e
    ```

2. Add a line to run your script at reboot:
    ```bash
    @reboot /usr/bin/python3 /path/to/send_daily_report.py
    ```

Make sure to replace `/path/to/send_daily_report.py` with the actual path to your script.

By following these steps, you'll have a Python script that automates sending daily email reports.
