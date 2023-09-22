Sure, I can help you create a simple donation modal that automatically pops up on the home page of your Next.js, Tailwind CSS, and TypeScript project using Headless UI. Since you mentioned a startup company for artists, I'll assume that you want to encourage users to support artists through donations. Here's a step-by-step guide with code:

1. First, make sure you have a Next.js project set up with Tailwind CSS and TypeScript.

2. Install the necessary dependencies:

   ```bash
   npm install @headlessui/react
   npm install react-query
   ```

3. Create a new component for the donation modal. You can name it `DonationModal.tsx`:

```tsx
// components/DonationModal.tsx
import React, { useState } from "react";
import { Dialog, Transition } from "@headlessui/react";

interface DonationModalProps {
  isOpen: boolean;
  onClose: () => void;
}

const DonationModal: React.FC<DonationModalProps> = ({ isOpen, onClose }) => {
  // You can add your donation form or content here

  return (
    <Transition.Root show={isOpen} as={React.Fragment}>
      <Dialog
        as="div"
        className="fixed inset-0 z-10 overflow-y-auto"
        onClose={onClose}
      >
        <div className="min-h-screen px-4 text-center">
          <Transition.Child
            as={React.Fragment}
            enter="ease-out duration-300"
            enterFrom="opacity-0"
            enterTo="opacity-100"
            leave="ease-in duration-200"
            leaveFrom="opacity-100"
            leaveTo="opacity-0"
          >
            <Dialog.Overlay className="fixed inset-0 bg-black opacity-30" />
          </Transition.Child>

          <span
            className="inline-block h-screen align-middle"
            aria-hidden="true"
          >
            &#8203;
          </span>

          <Transition.Child
            as={React.Fragment}
            enter="ease-out duration-300"
            enterFrom="opacity-0 scale-95"
            enterTo="opacity-100 scale-100"
            leave="ease-in duration-200"
            leaveFrom="opacity-100 scale-100"
            leaveTo="opacity-0 scale-95"
          >
            <div className="inline-block w-full max-w-md p-6 my-8 overflow-hidden text-left align-middle transition-all transform bg-white shadow-xl rounded-2xl">
              {/* Donation content goes here */}
              <p className="text-xl font-semibold">Support Artists</p>
              <p className="mt-2">
                Your support helps artists create more content.
              </p>

              {/* Donation form or any other content here */}
              {/* Example: */}
              <button
                className="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
                onClick={onClose}
              >
                Donate
              </button>
            </div>
          </Transition.Child>
        </div>
      </Dialog>
    </Transition.Root>
  );
};

export default DonationModal;
```

4. In your home page (`pages/index.tsx`), you can use the `DonationModal` component and automatically open it when the page loads for the first time. You can use React hooks and local storage to control the modal's visibility:

```tsx
// pages/index.tsx
import { useState, useEffect } from "react";
import DonationModal from "../components/DonationModal";

export default function Home() {
  const [isModalOpen, setIsModalOpen] = useState(false);

  // Check local storage to see if the modal has been closed previously
  useEffect(() => {
    const hasClosedModal = localStorage.getItem("hasClosedModal");
    if (!hasClosedModal) {
      setIsModalOpen(true);
    }
  }, []);

  const closeModal = () => {
    setIsModalOpen(false);
    // Save in local storage that the modal has been closed
    localStorage.setItem("hasClosedModal", "true");
  };

  return (
    <div>
      {/* Your home page content here */}
      <h1>Welcome to our Artist Support Platform</h1>

      <DonationModal isOpen={isModalOpen} onClose={closeModal} />
    </div>
  );
}
```

This code sets up a donation modal that will automatically appear on the home page when the user visits the site for the first time. After the user closes the modal, it won't appear again due to the use of local storage to track if it has been closed.

Remember to style the modal and add your donation form or content as needed.
