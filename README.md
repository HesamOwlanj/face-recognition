# face-recognition
# A Simple Face Recognizion Program with  CV2 &amp; Dlib Libraries
# This is a basic example, and in a real-world scenario, you would need a more extensive dataset for training




import cv2
import dlib

face_rec_model = dlib.face_recognition_model_v1("path_to_pretrained_model.dat")

image_path = "path_to_image.jpg"
image = cv2.imread(image_path)

face_detector = dlib.get_frontal_face_detector()

# Detect faces in the image
faces = face_detector(image)

shape_predictor = dlib.shape_predictor("path_to_shape_predictor_68_face_landmarks.dat")

known_face_embeddings = []
known_face_names = []

for face in faces:
    landmarks = shape_predictor(image, face)
    face_embedding = face_rec_model.compute_face_descriptor(image, landmarks)
    known_face_embeddings.append(face_embedding)

# Example: Create a test face embedding for comparison
test_face_embedding = [0.1, 0.2, 0.3, ...]

for i, known_embedding in enumerate(known_face_embeddings):
    # Use a suitable distance metric (e.g., Euclidean distance)
    distance = sum((a - b) ** 2 for a, b in zip(known_embedding, test_face_embedding)) ** 0.5

    # Set a threshold for face recognition
    threshold = 0.6  # Adjust as needed

    if distance < threshold:
        # Face recognized
        print(f"Recognized: {known_face_names[i]}")
        break
else:
    # Face not recognized
    print("Unknown face")

for face in faces:
    x, y, w, h = face.left(), face.top(), face.width(), face.height()
    cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)

# Display the image
cv2.imshow("Face Recognition", image)
cv2.waitKey(0)
cv2.destroyAllWindows()
