Certainly! Here's how you can implement a contract event listener in a Next.js TypeScript application using the `ethers` library:

1. **Install Dependencies**:

   Install the required packages:

   ```bash
   npm install ethers
   ```

2. **Create Event Listener**:

   In your component or page, you can use the `useEffect` hook to create and manage the event listener:

   ```tsx
   import React, { useEffect } from "react";
   import { Contract, providers } from "ethers";
   import { YourContractAbi } from "./YourContractAbi"; // Import your contract ABI

   const YourComponent: React.FC = () => {
     useEffect(() => {
       const provider = new providers.Web3Provider(window.ethereum);

       const contractAddress = "0xYourContractAddress"; // Replace with your contract address
       const contract = new Contract(
         contractAddress,
         YourContractAbi,
         provider
       );

       const eventName = "YourEventName"; // Replace with your event name
       const filter = contract.filters[eventName]();

       const eventListener = async () => {
         const events = await contract.queryFilter(filter);
         events.forEach((event) => {
           console.log("Event Data:", event.args);
           // Handle the event data as needed
         });
       };

       contract.on(filter, eventListener);

       // Clean up the event listener when the component unmounts
       return () => {
         contract.off(filter, eventListener);
       };
     }, []); // Empty dependency array means the effect runs only once on mount

     return <div>{/* Your component content */}</div>;
   };

   export default YourComponent;
   ```

3. **Integrate with Next.js**:

   You can integrate the `YourComponent` in your Next.js application as needed.

In this example, the `ethers` library is used to create a contract instance, set up an event filter, and listen for events using the `on` method. The event listener cleans up when the component unmounts.

Remember to handle the event data according to your use case and consider type safety when working with TypeScript.
