# 🪄 Invisibility Cloak — OpenCV

> *“It is not magic. It is computer vision.”*

Ever wondered what it would look like to wear **Harry Potter's Invisibility Cloak**?

This project is a fun little experiment with **OpenCV, HSV color detection, and image masking** that creates an invisibility-cloak effect using nothing more than a webcam and a piece of red cloth.

Wrap yourself in red, step in front of the camera, and watch the cloth — and everything underneath it — seemingly disappear. 🧙‍♂️✨

---

## ✨ The Idea

The trick is actually pretty simple:

Instead of making the person invisible, we make the **red cloak disappear** and replace it with a previously captured image of the background.

### 🪄 The magic happens in 4 steps

```text
        📷 Webcam
            │
            ▼
    Capture Empty Background
            │
            ▼
     Detect Red Cloak
       using HSV
            │
            ▼
    Create Binary Mask
       ┌────┴────┐
       ▼         ▼
   Keep the    Replace the
   normal      red region with
   pixels      background
       └────┬────┘
            ▼
       🪄 Final Frame
```

### 🔮 In Harry Potter terms

**1. `Prior Incantato` — Capture the Background**

Before entering the frame, the program captures the empty scene.

This image becomes the **background plate**.

**2. `Revelio` — Find the Cloak**

Each webcam frame is converted from BGR to **HSV color space**.

The program then looks for pixels belonging to the red color range.

**3. `Evanesco` — Make It Disappear**

A mask is created for the detected red pixels.

Those pixels are removed from the live frame and replaced with the corresponding pixels from the background image.

**4. `Mischief Managed` — Show the Result**

The processed frame is displayed in real time and simultaneously recorded to:

```text
output.avi
```

---

## 🧠 How It Works Technically

The project uses a few fundamental computer-vision concepts:

### 🎨 HSV Color Detection

Instead of detecting red directly in BGR, the frame is converted to HSV:

```python
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

Two ranges are used because red appears at **both ends of the HSV hue spectrum**.

```python
lower_red = np.array([0, 120, 50])
upper_red = np.array([10, 255, 255])

lower_red = np.array([170, 120, 70])
upper_red = np.array([180, 255, 255])
```

The two masks are combined to identify the cloak.

### 🧹 Mask Cleaning

Small imperfections in the detected mask are reduced using morphological operations:

```python
cv2.morphologyEx(mask, cv2.MORPH_OPEN, ...)
cv2.morphologyEx(mask, cv2.MORPH_DILATE, ...)
```

### 🪞 Background Replacement

The inverse mask keeps everything that isn't the cloak:

```python
mask_inv = cv2.bitwise_not(mask)
```

The original image is kept outside the cloak:

```python
res1 = cv2.bitwise_and(img, img, mask=mask_inv)
```

The previously captured background is placed where the cloak was detected:

```python
res2 = cv2.bitwise_and(background, background, mask=mask)
```

Finally, both are combined:

```python
finalOutput = cv2.addWeighted(res1, 1, res2, 1, 0)
```

And just like that...

**Evanesco. 🪄**

---

## 🛠️ Tech Stack

* 🐍 Python
* 👁️ OpenCV
* 🔢 NumPy
* 📷 Webcam
* 🎥 XVID video encoding

---

## 📦 Requirements

* Python 3.7+
* OpenCV
* NumPy
* A working webcam
* A red cloth / blanket / piece of fabric
* Reasonably consistent lighting

Install the dependencies:

```bash
pip install opencv-python numpy
```

---

## 🚀 Run the Project

Clone the repository:

```bash
git clone <your-repository-url>
cd <your-repository-name>
```

Run:

```bash
python invisibility_cloak.py
```

### 🧙‍♂️ Casting the Spell

**Step 1 — Start the program**

Give the camera a moment to initialize.

**Step 2 — Leave the frame**

For the first few seconds, stay out of view.

The program captures the empty background.

**Step 3 — Enter with the red cloak**

Walk into the frame wearing the red cloth.

**Step 4 — Watch the magic**

The red region is detected and replaced with the background.

**Step 5 — End the spell**

Press:

```text
q
```

The processed video will be saved as:

```text
output.avi
```

---

## 🎭 For the Best Invisibility Effect

The illusion works best when:

* 🔴 The cloth is a **solid, bright red**
* 💡 Lighting is reasonably consistent
* 🖼️ The camera remains stationary
* 🪑 The background doesn't change after calibration
* 👤 You don't move too quickly
* 🎨 There aren't many other red objects in the scene

### ⚠️ Why does it sometimes fail?

This isn't actually an invisibility spell.

The program assumes that **red = cloak**.

So if there's a red chair, red object, or red shirt somewhere else in the scene, the algorithm may decide that it's part of the cloak too.

Computer vision can be a little too literal. 😭

---

## 📁 Project Structure

```text
Invisibility-Cloak/
│
├── invisibility_cloak.py
├── requirements.txt
├── README.md
└── output.avi        # generated after running
```

---

## 💡 What I Learned

This project was a fun way to understand some of the basics of computer vision:

* Working with webcam frames
* Converting between color spaces
* HSV-based color segmentation
* Binary masks
* Morphological operations
* Bitwise image operations
* Background subtraction/replacement
* Real-time video processing
* Saving processed video using OpenCV

---

## 🪄 What's Actually Happening?

No spells.

No CGI.

No green screen.

Just:

```text
Webcam
   ↓
HSV Color Detection
   ↓
Red Mask
   ↓
Background Replacement
   ↓
OpenCV
   ↓
✨ "Invisibility" ✨
```

The real magic is just **pixels, masks, and a little bit of computer vision.**

---

## 🔮 Possible Future Improvements

The current version is intentionally simple, but it could be extended with:

* 🎨 Support for multiple cloak colors
* 🎯 Automatic color calibration
* 📍 Better mask refinement
* 🧍 Person/cloth segmentation
* 🎥 Better video output quality
* ⚡ More robust real-time processing
* 🪄 A Harry Potter-style UI and sound effects

---

## ⚡ Disclaimer

This project does **not** actually make you invisible.

Please do not test it by walking into traffic.

The Ministry of Magic has not approved this software. 🧙‍♂️

---

## 📜 License

MIT License

Feel free to fork it, modify it, experiment with it, and add your own magic.

> *“Any sufficiently advanced technology is indistinguishable from magic.”*

---

### ⭐ If you enjoyed this little experiment

Give the repository a ⭐ and try creating your own computer-vision magic.

**Mischief managed. 🪄**
