# xiaohongshu_janitor

Automating tasks on Xiaohongshu (RED) is a bit unique because the platform has incredibly strict **anti-bot and risk-control systems**. If a script moves too fast or behaves mechanically, your account can easily get restricted or banned.

Because of this, traditional web scraping (API requests) is highly discouraged. You want **UI automation**—tools that simulate a real human tapping, scrolling, and reading the screen.

Here are the best technologies to use, depending on your programming comfort level and whether you are targeting Android or iOS.

---

### Option 1: Auto.js / Hamibot (Easiest & Most Popular)

If you are using an **Android** device, this is the go-to stack in the automation community for apps like Xiaohongshu.

* **What it is:** JavaScript-based automation frameworks that run directly on your phone using Android's native **Accessibility Services**.
* **Why it's great:** You don't need to keep your phone plugged into a computer. It can read text directly on the screen (e.g., "0 posts") and simulate natural swipes and clicks.
* **Tech Stack:** JavaScript.

### Option 2: Python + `uiautomator2` (Best for Developers)

If you want to write a clean Python script from your PC while your Android phone is plugged in via USB.

* **What it is:** A Python wrapper for Google's native Android UIAutomator test framework.
* **Why it's great:** It is much faster and lighter than Appium. You can write simple logic like:
```python
# Conceptual logic
if d(text="暂无动态").exists or d(text="0作品").exists:
    # Click unfollow

```
*   **Tech Stack:** Python, `uiautomator2` library, ADB (Android Debug Bridge).

### Option 3: Appium (The Cross-Platform Standard)
If you absolutely must do this on **iOS** (or want a tool that supports both Android and iOS).

*   **What it is:** The industry-standard mobile testing framework.
*   **Why it's great:** Highly robust and supports multiple languages (Python, Java, JS).
*   **The Catch:** Setting it up for iOS requires a Mac, an Apple Developer account, and configuring WebDriverAgent. It's a massive headache for a quick personal project compared to Android tools.
*   **Tech Stack:** Python/JS, Appium Server, Node.js.

---

## How the Logic Code Would Work

Regardless of the tool you choose, your program will need to follow this visual logic loop:

<Sequence>

  <Step subtitle="Manual or Automated" title="Navigate to Following List">
    Open your profile, tap on "Following" (关注), and wait for the list to load.
  </Step>
  <Step subtitle="Human-like delays" title="Loop & Tap Each Profile">
    Read the names on the screen. Tap the first user's profile to open their homepage. Add a random delay of 1–2 seconds to look human.
  </Step>
  <Step subtitle="Text element inspection" title="Check Post Count">
    Look for specific UI text indicators. Xiaohongshu usually displays "0笔记" (0 notes) or shows an empty state illustration with text like "还没有发布笔记".
  </Step>
  <Step subtitle="Action step" title="Conditional Unfollow">
    If the text matches the empty state, tap the "Following" button to unfollow them. If they have posts, do nothing.
  </Step>
  <Step subtitle="State management" title="Go Back & Scroll">
    Simulate the system "Back" gesture to return to the main list. Scroll down slightly to bring the next batch of users into view, and repeat.
  </Step>
</Sequence>

---

## ⚠️ Vital Precautions for Xiaohongshu
If you build this, keep these three rules in mind to protect your account:

*   **Use Random Delays:** Never let a click happen at exact intervals (e.g., exactly every 2.0 seconds). Use a random number generator to pause anywhere from 1.5 to 4.5 seconds between actions.
*   **Set Daily Limits:** Do not try to unfollow 500 people in one sitting. Limit your script to run in small batches (e.g., 50 people per session), or the app will flag your device as a spam bot.
*   **OCR Fallback:** Xiaohongshu frequently updates its UI layout to break layout-inspector tools. If your script suddenly stops recognizing text elements, you may need to use a lightweight OCR library (like `PaddleOCR`) to literally "read" the screen pixels.

```
