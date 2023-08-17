Certainly! To achieve this, you'll need to generate share links that include the referral link and any message you want to add. Here's how you can modify the `ReferralShareComponent` to achieve this:

1. **Update the ReferralShareComponent:**

Update the `ReferralShareComponent.tsx` file in the `components` directory with the following code:

```jsx
// components/ReferralShareComponent.tsx
import React from "react";
import {
  FacebookShareButton,
  TwitterShareButton,
  RedditShareButton,
  WhatsappShareButton,
} from "react-share";
import {
  FacebookIcon,
  TwitterIcon,
  RedditIcon,
  WhatsappIcon,
} from "react-share";

interface ReferralShareComponentProps {
  referralLink: string;
}

const ReferralShareComponent: React.FC<ReferralShareComponentProps> = ({
  referralLink,
}) => {
  const shareMessage = "Check out this referral link!";

  return (
    <div className="flex flex-col items-center space-y-4">
      <div className="flex space-x-4">
        <FacebookShareButton url={referralLink} quote={shareMessage}>
          <FacebookIcon size={32} round={true} />
        </FacebookShareButton>
        <TwitterShareButton url={referralLink} title={shareMessage}>
          <TwitterIcon size={32} round={true} />
        </TwitterShareButton>
        <RedditShareButton url={referralLink} title={shareMessage}>
          <RedditIcon size={32} round={true} />
        </RedditShareButton>
        <WhatsappShareButton url={referralLink} title={shareMessage}>
          <WhatsappIcon size={32} round={true} />
        </WhatsappShareButton>
      </div>
      <div className="flex items-center space-x-2">
        <input
          type="text"
          value={referralLink}
          className="border rounded-md px-2 py-1 w-full focus:outline-none focus:ring focus:border-blue-300"
          readOnly
        />
        <button
          onClick={() => navigator.clipboard.writeText(referralLink)}
          className="bg-blue-500 text-white rounded-md px-4 py-2 hover:bg-blue-600 focus:outline-none focus:ring focus:border-blue-300"
        >
          Copy
        </button>
      </div>
    </div>
  );
};

export default ReferralShareComponent;
```

2. **Use the ReferralShareComponent:**

Open the `pages/index.tsx` file and use the `ReferralShareComponent` component as before:

```jsx
// pages/index.tsx
import React from "react";
import ReferralShareComponent from "../components/ReferralShareComponent";

const Home: React.FC = () => {
  const referralLink = "https://your-referral-link.com";

  return (
    <div className="min-h-screen flex items-center justify-center">
      <div className="max-w-md mx-auto p-4">
        <h1 className="text-2xl font-bold mb-4">Referral Share Component</h1>
        <ReferralShareComponent referralLink={referralLink} />
      </div>
    </div>
  );
};

export default Home;
```

3. **Start the Development Server:**

Run the development server:

```bash
npm run dev
```

Now, when you navigate to `http://localhost:3000`, you should see the `ReferralShareComponent` with Facebook, Twitter, Reddit, WhatsApp buttons, and a copy button for the referral link. Clicking the social media buttons will open a share dialog for the corresponding platform with the referral link and the predefined share message. Clicking the copy button will copy the referral link to the clipboard as before.
