#server
import os, cv2, socket, pickle, time

cap=cv2.VideoCapture(0)

# Create Socket
s=socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(("192.168.43.178",12345))
s.listen(5)

# Listening and waiting for connection
conn,addr = s.accept()
photo=None

while True:
    # Receive Photo
    data = conn.recv(945600)
    try:
        photo = pickle.loads(data)
        print("photo received at server ")
        cv2.imshow("CLIENT-SCREEN", photo)
        if cv2.waitKey(10) == 13:
            break
    except:
        print("Pickle error")

    #click and send photo
    stat,photo=cap.read()
    if stat:
        photo_data = pickle.dumps(photo)
        conn.sendall(photo_data)
    else:
        print("error capturing")

cv2.destroyAllWindows()
cap.release()
os.system("cls")
