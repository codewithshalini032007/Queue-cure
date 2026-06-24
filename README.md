Queue Cure '26 — Clinic Queue Management System


Built for the Queue Cure '26 Full Stack Hackathon on Wooble.



76% of India's 1.5 million clinics run on paper tokens and shouting. This app fixes that — a real-time queue system with a receptionist dashboard and a patient waiting room that sync instantly via WebSockets.


Live Demo

Deploy instructions below → get a public URL in under 5 minutes.


What It Does

ScreenWho uses itWhat they seeScreen 1 — ReceptionistDoctor's front deskAdd patients, call next token, set avg consultation time, clear queueScreen 2 — Patient waiting roomPatients in the clinicCurrent token being seen, tokens ahead, estimated wait time

Live sync — both screens update the moment "Call Next" is clicked. No refresh needed.


Tech Stack

LayerTechnologyBackendNode.js + ExpressReal-timeSocket.IOFrontendVanilla HTML/CSS/JSHostingRailway / Render (free)


Project Structure

queue-cure-26/
├── server.js          ← Socket.IO server (all business logic)
├── package.json
└── public/
    └── index.html     ← Both screens in one page (split view)


Socket Events Reference

Client → Server

EventPayloadDescriptionjoin-room—Join the shared clinic-queue roomadd-patient{ name: string }Add a patient to the queuecall-next—Advance queue, set current tokenset-avg-time{ minutes: number }Update avg consultation timeclear-queue—Reset everything

Server → Clients (broadcast to room)

EventPayloadDescriptionqueue-updated{ queue, current, avgTime }Full state sync — fires after every changetoken-called{ token, name }Notification when a token is called


Run Locally

Prerequisites


Node.js v16 or higher
npm


Steps

bash# 1. Clone or download this project
git clone https://github.com/YOUR_USERNAME/queue-cure-26.git
cd queue-cure-26

# 2. Install dependencies
npm install

# 3. Start the server
npm start

# 4. Open in browser
# http://localhost:3000

Open two browser tabs at http://localhost:3000:


Tab 1 = Receptionist view (left panel)
Tab 2 = Patient view (right panel)


Click "Call Next" in Tab 1 → Tab 2 updates instantly. That's your live sync demo.


Deploy to Railway (Free — Recommended)

Railway supports Socket.IO out of the box.

bash# 1. Push your code to GitHub
git init
git add .
git commit -m "Queue Cure '26 initial commit"
git remote add origin https://github.com/YOUR_USERNAME/queue-cure-26.git
git push -u origin main

# 2. Go to https://railway.app
# 3. Click "New Project" → "Deploy from GitHub repo"
# 4. Select your repository
# 5. Railway auto-detects Node.js and runs `npm start`
# 6. Click "Generate Domain" → get your public URL

Done. Share that URL as your Wooble submission link.


Deploy to Render (Free Alternative)

1. Go to https://render.com
2. New → Web Service → Connect GitHub repo
3. Build Command: npm install
4. Start Command: node server.js
5. Click "Create Web Service"
6. Wait ~2 min → get your public URL


How the Live Sync Works

Receptionist clicks "Call Next"
        ↓
  socket.emit('call-next')
        ↓
   Server: queue.shift() → state updates
        ↓
  io.to('clinic-queue').emit('queue-updated', state)
  io.to('clinic-queue').emit('token-called', { token, name })
        ↓
  Both screens receive events simultaneously
  → Both UIs re-render instantly

All clients join a single Socket.IO room (clinic-queue). One broadcast reaches everyone — no polling, no refresh.


Demo Script (for Wooble submission video)


Open the app in two browser windows side by side
Receptionist window: Add 3–4 patients by name
Show both screens updating the queue list simultaneously
Receptionist window: Click "Call Next"
Show the patient screen: token number jumps, notification pops up, wait time updates
Change avg consultation time — show wait estimates update live on both screens
Keep clicking "Call Next" until queue empties



Submission Checklist (Wooble)


 Working prototype link (deployed URL)
 Demo video (record the 2-window live sync demo above)
 GitHub repo link (make it public)
 README (this file)



Author

Built for Queue Cure '26 Hackathon on Wooble.
