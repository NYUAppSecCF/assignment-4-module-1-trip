# Assignment 4 - Module 1: Mobile Mess

## Introduction

Your organization has decided to create an Android application for students to purchase NYU GiftCards.
They hired a contractor to develop it — but the result was, unfortunately, a mess. 
Though your boss never confirmed which company was hired, you're almost certain it was ShoddyCorp’s Cut-Rate Contracting, a name infamous for poor software practices.

ShoddyCorp also provided a backend for the application, but that task has been handed off to another teammate.
You, however, have been asked to clean up and secure the Android app itself.

Thankfully, Kevin Gallagher (KG) reviewed the code and listed the key issues. It’s now your job to implement those changes and get the app ready for release.

---

## Part 1: Setting Up Your Environment

### Step 1: Clone the Repository

Use Git to clone the provided repository, as in previous assignments.

### Step 2: Install Android Studio

Install Android Studio from:

```
https://developer.android.com/studio/
```

**Important:** Install it on your **host machine**, not a Linux virtual machine. Android Studio works well on Windows (x86 only), Linux, macOS, and Chrome OS.  If you have a
different platform, we assume you have solutions in place when dealing with mainstream applications. 

Visual walkthrough (Mac-based, but generally applicable):  
👉 [Setup guide](https://imgur.com/a/noDTMNj)

### Step 3: Import the Project

1. Launch Android Studio
2. Choose **More Actions → Import Project (Gradle, Eclipse ADT, etc.)**
3. Navigate to the repository
4. Select the `GiftcardSite` folder inside the repository you cloned. 

✅ **Important:** Only select the `GiftcardSite` folder, or Android Studio will not configure the project correctly.

Accept any prompts (e.g., **Trust Gradle Project**) and let dependencies finish syncing.

### Step 4: Set Up the Android Emulator

1. Go to **Tools → Device Manager**
2. Click **Create Virtual Device**
3. Choose **Pixel 3a**
4. Select the **R system image** (Android 11, API Level 30, x86 ABI, Google Play)
5. If not downloaded, click **Download**, accept the license, and wait
6. Keep default settings:
   - Orientation: Portrait  
   - Graphics: Automatic  
   - Device Frame: Enabled  
7. Click **Finish**

The first emulator launch may take time. Be patient as the virtual device initializes.

### Step 5: Run the Application

Click the green **Run** (Play) button to build and launch the app in the emulator. Open the **Logcat** tab to view debug messages (from `Log.d()` statements).

## What to Submit (Part 1)

- At least **one signed Git commit**
- Use **GitHub Actions** to automatically check if the project compiles with Gradle

You do **not** need to write tests (but certainly good to do). As a tip: take a look at this CI workflow template:  
[Android GitHub Actions](https://github.com/actions/starter-workflows/blob/main/ci/android.yml)

Since we are using a self-hosted runner, include:

```yaml
- name: Setup Android SDK
  uses: android-actions/setup-android@v2
```

Read more in this reference: [Gradle CLI Documentation](https://docs.gradle.org/current/userguide/command_line_interface.html)

---

## Part 2: It’s All About Intent

### 2.1 Understanding the Difference

Android uses **Intents** to transition between components or access external apps.

Examine:
- `SecondFragment.kt` (lines 69–73)
- `ThirdFragment.kt` (lines 68–70)

Answer the following reflection questions (not graded), but **critical for understanding Android security**.  
**Hint: It's good to 'assess' your understanding of this content**

1. What are the two types of Intents?
2. Which type is generally more secure?
3. What type of Intent is used in `SecondFragment.kt`?
4. What type is used in `ThirdFragment.kt`?
5. Which Intent is considered the "proper" implementation?
6. What does "proper" even mean in the context of secure Android development?  
   **Hint:** Consider how attackers might take advantage of improperly scoped or overly permissive Intents.

👉 Once you identify the insecure usage, **fix it.**

### 2.2 Shutting Out the World

Currently, other apps can launch Activities in the GiftCard app using Intents.

**This is not desired.** Your company does **not** anticipate the need for any external app to interact with your Activities. They have asked you lock things down!

Make the necessary changes to `AndroidManifest.xml` to **prevent external apps from launching any Activities** in your app.

**Hint:** Look at `exported` attributes and intent filters.

---

## Part 3: Can You Read Me Out There?

Right now, this app **does not use HTTPS** to communicate with the backend API — a critical failure.

Your task is to **secure all API communication** by using HTTPS instead of HTTP.

Update the following files:

1. `SecondFragment.kt`  
2. `ThirdFragment.kt`  
3. `CardScrollingActivity.kt`  
4. `ProductScrollingActivity.kt`  
5. `UseCard.kt`  
6. `GetCard.kt`  
7. `CardRecyclerViewAdapter.kt`  
8. `RecyclerViewAdapter.kt`  
9. `Reporter.kt`  
10. `strings.xml`

**Hint:** This is not a complicated task. You should only be replacing existing strings and modifying URLs — no need for external libraries or major rewrites. Don't over think, focus on the task at hand!

---

## Part 4: Oops, Was That Card Yours?

There is a vulnerability in the REST API that allows users to **use gift cards that don’t belong to them**.

Examine the following files:

1. `UseCard.kt`  
2. `CardInterface.kt`

**Hint:** Think about how the application is telling the server which card to use. Could that data be manipulated?

Use tools like `curl` or Python’s `requests` to interact directly with the API.

🛑 **You are not required to fix this vulnerability.**
As you dig deeper, you should start to understand:

- Why this is happening
- Why **client-side code cannot fix it**
- What kind of backend validation is missing

**Hint:** It is a good thing to 'assess' your understanding of the role of client versus server side code in application development! We will be testing this understanding in the Assessment. 

---

## Part 5: Privacy is Important

This app unnecessarily collects data about users — a behavior that's **common** in modern mobile apps and deeply problematic. Luckily, you are a champion of privacy rights!

Remove any functionality that:

- Collects metrics
- Uses sensors
- Requests unnecessary permissions

Focus on the following files:

1. `AndroidManifest.xml`
2. `UserInfo.kt`
3. `CardScrollingActivity.kt`
4. `ProductScrollingActivity.kt`

**Hint:** The only permissions this app should request are those strictly needed to buy, browse, and use gift cards.

---

## What to Submit

The repository should contain all the files of the Android project, write-ups are not required for this assignment! Do be mindful of diskspace usage as always. 

Please **only submit a file called `git_link.txt`** that contains the name of your repository to **Gradescope**.

For example, if your repo is located at 'h<span>ttps:</span>//github.com/NYUAppSec__/assignment-4-module-1-exampleaccount', you would submit a text file named `git_link.txt` with one single line that contains <ins><b>only</b></ins> the following:

    assignment-4-module-1-exampleaccount

When you enter your path keep in mind that each semester is different, the above is just an example. Pay attention to your specific repo path.

Remember that <b>Gradescope is not instant</b>. Especially if we have to look into past GitHub action runs. We have a timeout set for 10 minutes, almost all well running code will complete within 5 minutes. Wait for it to complete or timeout before trying to re-run.

---

### Ready for Grading

Feel free to start submitting on gradescope to see how you would score. Once you want to lock in your grade push the `assign4mod1handin` tag with the following:

To submit this part, push the `assign4mod1handin` tag with the following:

```commandline
git tag -a -m "Completed assign4 mod1." assign4mod1handin
git push origin main
git push origin assign4mod1handin
```
**DO NOT PUSH THIS TAG UNTIL YOU WANT TO BE GRADED**

There is only one module here!

| ⚠️ **Use the NYU self-hosted runner** |
|---|
| To receive credit, your workflow (if you have one) **must run on our NYU self-hosted runner**. Using GitHub-hosted runners (e.g., `ubuntu-latest`) may incur charges and will earn a **zero**.<br><br>**✅ Correct:** `runs-on: self-hosted`<br>**❌ Do NOT use:** `runs-on: ubuntu-latest`<br><br>**Quick check:** In the job details, the **Runner** must show `self-hosted`. |

---

## Concluding Remarks

Even after your updates, this app remains imperfect. The core design is flawed, and security depends just as much on the server as it does on the client.

Still, you’ve taken an important step forward. This assignment teaches you:
- Secure Android development
- Intent misuse
- Insecure data transmission
- Broken access control models
- Privacy-respecting app design

And yes — we will be asking you follow-up questions on this!
