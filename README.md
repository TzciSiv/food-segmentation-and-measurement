# 📸 Food Calorie Estimation with Real-World Size Awareness

This project builds a **more accurate food calorie estimation system** by solving a big problem in existing calorie apps:  
👉 **photos don’t show real size**.

Instead of guessing how big food is from an image, this system **measures it** using a known reference object and then lets an AI estimate calories using the true portion size.

---

## 🚨 Problem

Most calorie-tracking apps:
- Use only images
- Guess food size from appearance
- Often make large mistakes because food sizes vary a lot  

Example:
- A strawberry can be tiny or huge  
- A chicken breast may look small but weigh 150g+  

If size is wrong → calories are wrong.

---

## 💡 Key Idea (Core Innovation)

Place a **standard playing card** next to the food when taking a photo.

Why this works:
- A playing card has a **known height (88 mm)**
- The model uses it as a **real-world ruler**
- Pixel size → real size → accurate portion estimation

---

## 🧠 System Overview

1. **Segmentation (YOLO11x-seg)**
   - Detects and segments:
     - `food`
     - `card`

2. **Real-World Measurement**
   - Uses the known card height to compute a **pixel-to-mm scale**
   - Measures the food’s real dimensions

3. **LLM-Based Calorie Estimation**
   - Cropped food image + real size sent to an LLM
   - LLM identifies food type and estimates calories **without guessing size**

---

## 🏗️ Pipeline Structure

# 📸 Food Calorie Estimation with Real-World Size Awareness

This project builds a **more accurate food calorie estimation system** by solving a big problem in existing calorie apps:  
👉 **photos don’t show real size**.

Instead of guessing how big food is from an image, this system **measures it** using a known reference object and then lets an AI estimate calories using the true portion size.

---

## 🚨 Problem

Most calorie-tracking apps:
- Use only images
- Guess food size from appearance
- Often make large mistakes because food sizes vary a lot  

Example:
- A strawberry can be tiny or huge  
- A chicken breast may look small but weigh 150g+  

If size is wrong → calories are wrong.

---

## 💡 Key Idea (Core Innovation)

Place a **standard playing card** next to the food when taking a photo.

Why this works:
- A playing card has a **known height (88 mm)**
- The model uses it as a **real-world ruler**
- Pixel size → real size → accurate portion estimation

---

## 🧠 System Overview

1. **Segmentation (YOLO11x-seg)**
   - Detects and segments:
     - `food`
     - `card`

2. **Real-World Measurement**
   - Uses the known card height to compute a **pixel-to-mm scale**
   - Measures the food’s real dimensions

3. **LLM-Based Calorie Estimation**
   - Cropped food image + real size sent to an LLM
   - LLM identifies food type and estimates calories **without guessing size**

---

## 🧹 Data Preprocessing

- **FoodSeg103**
  - 103 food classes → collapsed into **1 class: `food`**
- **Card Dataset**
  - 52 card identities → collapsed into **1 class: `card`**
- Only images with **exactly one visible card** are kept
- All segmentation masks converted to **YOLO polygon format**

Final classes:
- `0 = food`
- `1 = card`

---

## 🏋️ Model Training

- **Model:** YOLO11x-seg
- **Image Size:** 640 × 640
- **Epochs:** 10
- **Batch Size:** 16–32
- **Optimizer:** AdamW
- **Pretrained Weights:** Yes
- **Augmentations:**
  - Horizontal flip
  - Color jitter
  - Resizing

Model size:
- 203 layers
- ~62 million parameters

---

## 📊 Results

- **Box mAP:** ~0.95  
- **Mask mAP:** ~0.95  

Observations:
- Card segmentation is extremely accurate (simple, consistent shape)
- Food segmentation performs strongly despite high variation

Real-world size recovery:
- Measured food dimensions closely match actual object sizes
- Enables stable and realistic calorie estimates

---

## ✅ Why This Works Better

- LLMs are **bad at guessing physical size**
- Vision models are **good at precise measurement**
- This system lets:
  - YOLO handle geometry
  - LLM handle reasoning

Each tool does what it’s best at.

---

## 🌍 Applications

- Food calorie tracking
- Diet and nutrition monitoring
- Any task requiring **real-world measurement from images**

---

## 🏁 Summary

> By adding a simple reference object, this system turns a photo into a measuring tool — making calorie estimation far more accurate and reliable.
