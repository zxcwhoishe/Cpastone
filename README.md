import cv2

# GStreamer pipeline for UDP stream from OpenHD
gst_pipeline = (
    "gst-launch-1.0 udpsrc port=5800 ! application/x-rtp,encoding-name=H264 ! rtph264depay ! avdec_h264 ! videoconvert ! appsink"
)



# OpenCV VideoCapture using GStreamer pipeline
cap = cv2.VideoCapture(gst_pipeline, cv2.CAP_GSTREAMER)
#cap = cv2.VideoCapture("gst-launch-1.0 udpsrc port=5800 ! application/x-rtp,encoding-name=H264 ! rtph264depay ! avdec_h264 ! videoconvert ! autovideosink", cv2.CAP_GSTREAMER)

if not cap.isOpened():
    print("Failed to open video streams.")
    exit()

while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame.")
        break

    # Display video using OpenCV
    cv2.imshow('OpenHD Live Stream', frame)

    # Press 'q' to exit
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
