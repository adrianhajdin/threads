A fullstack Next.js 13 Meta Threads application 😃

# Requirements

1. Setup Next.js
   ```bash
   npx create-next-app@latest
   ```

   On installation, you'll see the following prompts:
   ```bash
   What is your project named? threads
   Would you like to use TypeScript? Yes
   Would you like to use ESLint? Yes
   Would you like to use Tailwind CSS? Yes
   Would you like to use `src/` directory? No
   Would you like to use App Router? (recommended) Yes
   Would you like to customize the default import alias? No
   ```
   
2. Setup Eslint & Prettier
   Setup eslint & prettier by following this guide (our guide) - [here](https://www.notion.so/jsmastery/Setup-ESLint-Prettier-in-VS-Code-56e50002b82b45b88e36265c5eafbb24?pvs=4)
   
4. Create an Account on [Clerk](https://clerk.com/)
   * Configure the Project by Selecting different auth options, i.e.,
     ![Screenshot 2023-07-23 at 4 51 50 PM](https://github.com/TidbitsJS/threads/assets/67959015/060152e4-d684-4809-895a-b6bc53121a65)
     * Email username password
       ![Screenshot 2023-07-23 at 4 48 06 PM](https://github.com/TidbitsJS/threads/assets/67959015/fba2ead7-9885-4b1c-a29d-aeea304c6936)
     * Google & Github as Social auth connections
       ![Screenshot 2023-07-23 at 4 50 17 PM](https://github.com/TidbitsJS/threads/assets/67959015/52409a13-ce97-4e82-90ff-d3d44f4bf07f)
   * Get the Clerk Keys from Home
     ![Screenshot 2023-07-23 at 4 53 01 PM](https://github.com/TidbitsJS/threads/assets/67959015/4ca5fe7f-14df-49d1-8504-fcd98760889d)
   * Enable Organization Feature
     ![Screenshot 2023-07-23 at 4 53 28 PM](https://github.com/TidbitsJS/threads/assets/67959015/10d2aa31-3a6d-4cf3-a066-74d5cbfd9779)
     Or, from here,
     ![Screenshot 2023-07-23 at 4 54 01 PM](https://github.com/TidbitsJS/threads/assets/67959015/29d74e99-cc73-464c-a97b-7cbdce5e429a)

   [Install Clerk](https://clerk.com/docs/nextjs/get-started-with-nextjs#install-clerk-nextjs) inside the application,
   ```bash
   npm install @clerk/nextjs
   ```

   To customize the organization component, we'll need [Clerk Theme](https://clerk.com/docs/nextjs/appearance-prop):
   ```
   npm install @clerk/themes
   ```

   Create endpoint and do not forget to setup the middleware in order for the Clerk to work:
   ```js
   // Resource: https://clerk.com/docs/nextjs/middleware#auth-middleware
   // Copy the middleware code as it is from the above resource

   import { authMiddleware } from "@clerk/nextjs";

   export default authMiddleware({
      // An array of public routes that don't require authentication.
      publicRoutes: ["/", "/api/webhook/clerk"],

      // An array of routes to be ignored by the authentication middleware.
      ignoredRoutes: ["/api/webhook/clerk"],
   });

   export const config = {
      matcher: ["/((?!.*\\..*|_next).*)", "/", "/(api|trpc)(.*)"],
   };
   ```

6. Create an Account on [UploadThing](https://uploadthing.com/)
   Setup a new project and get the keys from here
   ![Screenshot 2023-07-23 at 5 09 52 PM](https://github.com/TidbitsJS/threads/assets/67959015/bac97fcf-d006-468f-a203-2dba32cd09ca)

   Before starting the integration of UploadThing, make sure to install:
   ```bash
   npm install uploadthing @uploadthing/react
   ```

8. Create a new Database on [MongoDB](https://www.mongodb.com/)
9. Setup [shadcn](https://ui.shadcn.com/docs/installation/next) CLI
   ```bash
   npx shadcn-ui@latest init
   ```

   The configs should be,
   ```bash
   Would you like to use TypeScript (recommended)?  yes
   Which style would you like to use? › Default
   Which color would you like to use as base color? › Slate
   Where is your global CSS file? › › app/globals.css
   Do you want to use CSS variables for colors? › no
   Where is your tailwind.config.js located? › tailwind.config.js
   Configure the import alias for components: › @/components
   Configure the import alias for utils: › @/lib/utils
   Are you using React Server Components? › yes
   ```

   The final config should match this:
   ![Screenshot 2023-07-23 at 5 14 28 PM](https://github.com/TidbitsJS/threads/assets/67959015/23e41195-bb05-41fc-ab64-ff6c93e55424)

   From here, we can install the things that we need.
   Suppose, we need an Input element. Visit [Shadcn docs](https://ui.shadcn.com/docs/components/input) and find the command to download it in the repo:
   ```bash
   npx shadcn-ui@latest add input
   ```

   A similar process will go for,
   * [Button](https://ui.shadcn.com/docs/components/button)
   * [Tab](https://ui.shadcn.com/docs/components/tabs)
   * [Form](https://ui.shadcn.com/docs/components/form)
   * [Textarea](https://ui.shadcn.com/docs/components/textarea)


10. WebHooks
   We can handle the webhooks part without ngrok. We can write the api endpoint for it and deploy it on vercel. We can then set that URL in the Clerk Dashboard, which will call it every time a event happens

   Go to the Clerk Dashboard, Navigate to the WebHooks, and create a new endpoint:
   ![Screenshot 2023-07-23 at 9 09 02 PM](https://github.com/TidbitsJS/threads/assets/67959015/b4e1f1a0-a289-4227-be66-0fe6e6a2ba05)

   In the Filter event sections, tick the events we want to listen, i.e.,
   ```js
   "organization.created"
   "organizationInvitation.created"
   "organizationMembership.created"
   "organizationMembership.deleted"
   "organization.updated"
   "organization.deleted";
   ```

   After setting up the endpoint, you'll see the Signing Secret (for webhook verification) on the right side. Copy it.
  
   ![Screenshot 2023-07-23 at 9 11 33 PM](https://github.com/TidbitsJS/threads/assets/67959015/64f14c7d-e059-41dd-9160-309a3a80d885)

   Create a new env and paste it inside the Vercel Environment variables:
   ```bash
   NEXT_CLERK_WEBHOOK_SECRET="whsec_+Cmn4fUE8k5cqdEGC5iPc+BYCTMySE9I"
   ```

   Scroll down on the same page and you'll see the list of attempted events and logs associated with it
   ![Screenshot 2023-07-23 at 9 13 16 PM](https://github.com/TidbitsJS/threads/assets/67959015/53bd01e7-c21f-4053-b969-ceb33a09717b)

   Before getting started with webhook api setup, make sure to install svix:
   ```bash
   npm install svix
   ```


## Packages

We will need to install a couple of packages. Here is a complete list except for shadcn components (as we'll install them as we go, through its CLI)
```bash
npm install @clerk/nextjs @clerk/themes @uploadthing/react mongoose svix uploadthing
```

## Things to Provide

We can start from scratch and maybe copy paste few of these things as we go:

1. Tailwind CSS Config - [file](/tailwind.config.js)
2. Global CSS styles - [file](/app/globals.css)
3. Some utility functions - [file](/lib/utils.ts)
4. Constants - [file](/constants)
5. Images - [File](/public/assets)

#

# Process

Here is a short process on how we can approach the development of the website 

## Clerk
   - Setup Clerk & Configure the features and UI
   - Create [SignUp](https://clerk.com/docs/nextjs/signup#embedding-a-sign-up-component) and [SignIn](https://clerk.com/docs/nextjs/signin#embedding-a-sign-in-component) pages
   - Create the [auth middleware](https://clerk.com/docs/nextjs/middleware#auth-middleware) file
   - Since we have two different root layouts, make sure the wrap them inside the ClerkProvider, i.e.,
     - [Auth Layout](/app/(auth)/layout.tsx)
     - [Root Layout](/app/(root)/layout.tsx)
   - Clerk Customization
     - Primary Color: #877EFF
     - Background Color: #000000
     - Font Color: #101012
     - Button Font Color: #FFFFFF
![Screenshot 2023-07-26 at 6 11 00 PM](https://github.com/TidbitsJS/threads/assets/67959015/0355fdaf-4767-494b-bf18-147443c7a526)

Now the auth should work all well

## Uploadthing
   - Create an account and install the necessary packages
   - Create an API endpoint for it, i.e.,
     - [Core](/app/api/uploadthing/core.ts)
     - [route](/app/api/uploadthing/route.ts)

     It's the convention. Corresponding resource links have been mentioned in both files
   - Create a function to set up the function calls for the uploadthing in the lib folder, i.e., [uploadthing](/lib/uploadthing.ts)

Now you can import these functions from this file anywhere that is needed 

## Webhooks
   - Enable webhook in the Clerk Dashboard
   - Install the svix package
   - Create an API endpoint
     - We found the document link on how to create a webhook [here](https://github.com/clerkinc/clerk-nextjs-examples/blob/main/examples/widget/pages/api/webhooks/user.ts)
     - And an updated one [here](https://github.com/perkinsjr/Clerk-App-Router-Webhooks/blob/main/src/app/api/webhook/route.ts)
   - Deploy the application after writing the API endpoint code for the webhook
   - Get the deployed link and create a new endpoint in the Clerk Dashboard

Now you can try firing some events, i.e., creating an organization, adding a member, and it should work. We can check the logs on deployed platform for that event or via the Clerk Dashboard
  
