# How to Download PDFs from SCORM-Based E-Learning Platforms

> Useful when a course platform doesn't provide a download button for notes/slides, but the content is a PDF loaded in the browser.

---

## When This Works

- SCORM-based LMS platforms (Canvas, Moodle, Blackboard, corporate portals)
- PDFs that load inside an embedded course player
- Any browser-loaded file with no download option

---

## What You Need

- Google Chrome or any Chromium-based browser (Edge works too)
- Access to the course/module you want to download

---

## Step-by-Step

### 1. Open the Course Module
Launch the SCORM module as you normally would. Let it fully load.

### 2. Open DevTools
Press `F12` (or `Ctrl + Shift + I`) to open Chrome DevTools.

### 3. Go to the Network Tab
Click the **Network** tab at the top of DevTools.

### 4. Filter for PDFs
In the filter box, type:
```
.pdf
```

### 5. Reload or Navigate the Module
Interact with the module (click through slides, go to next page).  
The PDF request will appear in the Name column.

### 6. Copy the PDF URL
Click on the PDF filename in the Name column → go to **Headers** tab → copy the full **Request URL**.

It will look something like:
```
https://platform.com/vault/abc123/.../ModuleName.pdf
```

### 7. Open the Console Tab
Click the **Console** tab in DevTools.

### 8. Enable Pasting (First Time Only)
Chrome blocks pasting by default. To enable it:
1. Click inside the Console input
2. **Manually type** `allow pasting` and press `Enter`
3. You can now paste freely

### 9. Run the Download Script
Paste this into the Console and press `Enter`:

```javascript
fetch('PASTE_YOUR_PDF_URL_HERE')
  .then(r => r.blob())
  .then(blob => {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = 'notes.pdf';
    a.click();
  });
```

Replace `PASTE_YOUR_PDF_URL_HERE` with the URL you copied in Step 6.  
Replace `notes.pdf` with a meaningful filename if you want.

### 10. File Downloads
The PDF will download automatically to your default Downloads folder.

---

## Repeat for Other Modules

For each new module:
1. Navigate to it in the course player
2. A new PDF request will appear in the Network tab
3. Copy the new URL
4. Run the same fetch script with the new URL

---

## Why This Works

SCORM platforms protect content by requiring an active session — direct URL access is blocked. However, the browser itself loads the PDF to display it. The `fetch` script re-requests the file **within the same session context**, then triggers a local download using a Blob URL.

---

## Notes

- This only works while your browser session is active (logged in)
- Each PDF URL may be session-specific and expire after logout
- This is for personal study use — always respect your platform's terms of service

---

*Tested on: Chrome 149 | Canvas LMS + ContentController SCORM player*
