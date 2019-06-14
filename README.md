# backupScript
################################################################################
#       Script Writter:Nishat Ahmed                                            #
#       Script Purpose:For backup of data                                      #
#       Script Working:This script sends specified file through E-mail         #
################################################################################
#required libraries
import email
import smtplib
from email.mime.multipart import MIMEMultipart
from email import encoders
from email.mime.base import MIMEBase
from email.mime.text import MIMEText

again = 1
while again:
    #creditentials required
    fromaddr = "abc@hotmail.com"
    toaddr = "xyz@hotmail.com"
    password = "password"

    #input for name of file and its path
    print('enter name of the file to send:')
    filename = str(input())

    print('enter path of the file')
    fileToSend = str(input())
    fileToSend = '/root' + fileToSend + '/' + filename

    body = 'type body of mail here!'

    print('Processing Please Wait....')

    msg = MIMEMultipart() #object of mime multipart
    msg['From'] = fromaddr
    msg['To'] = toaddr
    msg['Subject'] = "Backup"

    # attach the body with the msg instance
    msg.attach(MIMEText(body,'plain'))

    # open the file to be sent
    attachment = open(fileToSend, 'rb')

    # instance of MIMEBase and named as p
    p = MIMEBase('application', 'octet-stream')

    # To change the payload into encoded form
    p.set_payload((attachment).read())

    # encode into base64
    encoders.encode_base64(p)

    p.add_header('Content-Disposition', "attachment; fileToSend= "+ fileToSend)

    # attach the instance 'p' to instance 'msg'
    msg.attach(p)

    #sending text as a string
    text = msg.as_string(p)

    #to hotmail ID(first parameter is domain and second parameter is port number)
    s = smtplib.SMTP("smtp.live.com",587)

    #Puts connection to SMTP server in TLS mode
    s.starttls()

    #logging in ID
    s.login(fromaddr, password)

    #sending mail
    s.sendmail(fromaddr, toaddr, msg.as_string())

    s.quit()
    print('Mail successfully send...')
    print('Do you want to send another mail ? (0 = False/1 = True)')
    again = int(input())
