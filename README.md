# shibora-auth
// .env.local (ไม่ควรเผยแพร่แบบนี้ในโปรเจกต์จริง)
TWITTER_CLIENT_ID=your-client-id=TXY4UVVhLXNZb1ZNOHYwNWtMSTg6MTpjaQ
TWITTER_CLIENT_SECRET=your-client-secret=iUOV30QHosLATLdt3R7AmK_WDOb2QXd_4nImPnaItQVzHVxL5P
TWITTER_REDIRECT_URI=https://your-vercel-url.vercel.app/api/auth/callback

// package.json
{
  "name": "shibora-login",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "latest",
    "react": "latest",
    "react-dom": "latest",
    "axios": "^1.6.0"
  }
}

// pages/api/auth/twitter.ts
import type { NextApiRequest, NextApiResponse } from 'next';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  const client_id = process.env.TWITTER_CLIENT_ID!;
  const redirect_uri = process.env.TWITTER_REDIRECT_URI!;
  const state = "shibora-login";

  const twitterAuthUrl = `https://twitter.com/i/oauth2/authorize?response_type=code&client_id=${client_id}&redirect_uri=${encodeURIComponent(
    redirect_uri
  )}&scope=tweet.read%20users.read&state=${state}&code_challenge=challenge&code_challenge_method=plain`;

  res.redirect(twitterAuthUrl);
}

// pages/index.tsx
import React from "react";

export default function Home() {
  return (
    <div style={{ padding: "2rem", textAlign: "center" }}>
      <h1>Shibora: The First 1,000 Thinkers</h1>
      <p>Login with X (Twitter) to join our philosophical journey.</p>
      <a
        href="/api/auth/twitter"
        style={{
          display: "inline-block",
          marginTop: "20px",
          padding: "10px 20px",
          backgroundColor: "#1DA1F2",
          color: "#fff",
          borderRadius: "5px",
          textDecoration: "none",
        }}
      >
        Login with Twitter
      </a>
    </div>
  );
}
