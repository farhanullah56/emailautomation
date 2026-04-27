# emailautomation
import smtplib
from datetime import date
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.base import MIMEBase
from email import encoders

sender_email='farhanullah0056@gmail.com'
passward='nsmv mifb lifx kwqg'

with open('D:/Data science/automation/emailauto/emails.txt') as file:
    emails=[email.strip() for email in file.readlines()]

subject=f'The report of {date.today()}'
email_string=', '.join(emails)
messege=MIMEMultipart()

messege['From']=sender_email
messege['To']=email_string
messege['Subject']=subject

with open('D:/Data science/automation/emailauto/new.txt','rb') as attach:
    data=attach.readlines()
    part=MIMEBase('application','octet-stream')
    part.set_payload(attach.read())
    encoders.encode_base64(part)
    part.add_header('Content Disposition','attachment; filename=new.txt')

body=f'There are{len(data)-1} complains in today file.'
messege.attach(MIMEText(body,'plain'))
messege.attach(part)
messege_string=messege.as_string()
    
with smtplib.SMTP('smtp.gmail.com',587) as connection:
    connection.starttls()
    connection.login(user=sender_email,password=passward)
    connection.sendmail(from_addr=sender_email,to_addrs=emails,msg=messege_string)
