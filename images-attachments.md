Add images and attachments to chat

---

### 000

We are going to start with supporting just images to the input for new projects. This is on the landing page. It has the placeholder text "Describe the app you want to build..."

It has an add attachments button that currently does nothing.

---

### 001

The attachments preview is failing on the input box

```
<img alt="Attachment" src="blob:http://localhost:5173/484f718a-2467-42cd-85f1-b1f9758ff3de" style="width: 100%; height: 100%; object-fit: cover;">
```

This is the rendered html element

---

### 002

The blob still shows a broken image? 

1. Can we debug the blob URL outside the browser context? Probably not, we need to double down on debugging before speculative fixes
2. 

---

### 003

Logs

```
ChatInput.tsx:84 Files selected: 1
ChatInput.tsx:95 Processing image: Screenshot 2026-01-22 at 18.06.50.png image/png 316934
ChatInput.tsx:100 Created blob URL: blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38 for file: Screenshot 2026-01-22 at 18.06.50.png
ChatInput.tsx:105 New attachments to add: 1
ChatInput.tsx:110 Updated attachments state: 1
ChatInput.tsx:110 Updated attachments state: 1
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38
localhost/:1 Refused to load the image 'blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38' because it violates the following Content Security Policy directive: "img-src 'self' data: res: https:".

ChatInput.tsx:230 Image failed to load: blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38 SyntheticBaseEvent {_reactName: 'onError', _targetInst: null, type: 'error', nativeEvent: Event, target: img, …}
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38
ChatInput.tsx:206 Rendering attachment: 9z5qljm blob:http://localhost:5173/dfd28bf5-6e7c-435b-add4-2d790ed9bf38

```

---

### 004

Claude code is not receiving the attachments.

Refer to the console logs at `bfloat-kb/pg/console.txt` to see if we can catch anything. If not, let's debug first what's going on before making speculative fixes.

---

### 005

I don't see any of our debug logs

**Electron browser console:** /Users/v1b3m/Dev/bfloat/bfloat-ide/bfloat-kb/pg/console.txt
**IDE Node Console:** /Users/v1b3m/Dev/bfloat/bfloat-ide/bfloat-kb/pg/ide-node-console.txt

