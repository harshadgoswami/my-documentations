Sure, here's an example of how you can implement a contract event listener in a Next.js TypeScript application using the `useEffect` hook:

1. **Install Dependencies**:

   Install the required packages:

   ```bash
   npm install web3 @types/web3
   ```

2. **Create Event Listener**:

   In your component or page, you can use the `useEffect` hook to create and manage the event listener:

   ```tsx
   import React, { useEffect } from "react";
   import Web3 from "web3";
   import { YourContractAbi } from "./YourContractAbi"; // Import your contract ABI

   const YourComponent: React.FC = () => {
     useEffect(() => {
       const web3 = new Web3(Web3.givenProvider); // Use an appropriate provider

       const contractAddress = "0xYourContractAddress"; // Replace with your contract address
       const contract = new web3.eth.Contract(YourContractAbi, contractAddress);

       const eventName = "YourEventName"; // Replace with your event name
       const event = contract.events[eventName]();

       event.on("data", (eventData: any) => {
         console.log("Event Data:", eventData);
         // Handle the event data as needed
       });

       event.on("error", (error: Error) => {
         console.error("Event Error:", error);
         // Handle the error if any
       });

       // Clean up the event listener when the component unmounts
       return () => {
         event.removeAllListeners();
       };
     }, []); // Empty dependency array means the effect runs only once on mount

     return <div>{/* Your component content */}</div>;
   };

   export default YourComponent;
   ```

3. **Integrate with Next.js**:

   You can integrate the `YourComponent` in your Next.js application as needed.

This example demonstrates how to create an event listener using the `useEffect` hook. It starts listening when the component mounts and cleans up when the component unmounts.

Remember that in a real-world scenario, you would likely want to handle the event data more effectively and update the component's state or perform other actions based on the event. Additionally, you may need to consider type safety for TypeScript when working with event data.
