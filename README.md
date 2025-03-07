Implementing Google and GitHub authentication from NexAuth.js using Next.js stack

Start with a fresh project
```bash
npx create-next-app@latest
```

Install Next-Auth 
```bash
npm i next-auth
```

add a file route in the app by ``` api/auth/[...nextauth]/route.js or ts ```

Inside ``` route.js ``` add the below code

``` bash

import NextAuth from "next-auth";
import GitHubProvider from "next-auth/providers/github";
import GoogleProvider from "next-auth/providers/google"

const handler = NextAuth({
  providers: [
    GitHubProvider({
      clientId: process.env.GITHUB_ID,
      clientSecret: process.env.GITHUB_SECRET,
    }),
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET
    })
  
  ]
});

export { handler as GET, handler as POST };


```
In ```page.js``` add the below code

```bash

"use client"
import { useSession, signIn, signOut } from "next-auth/react"

export default function Component() {
  const { data: session } = useSession()
  if(session) {
    return <>
      Signed in as {session.user.email} <br/>
      <button onClick={() => signOut()}>Sign out</button>
    </>
  }
  return <>
    Not signed in <br/>
    <button onClick={() => signIn()}>Sign in</button>
  </>
}

```

create a new file independently ```.env.local```

```bash 
GITHUB_ID=your-secret-string
GITHUB_SECRET=your-secret-string
GOOGLE_CLIENT_ID=your-secret-string
GOOGLE_CLIENT_SECRET=your-secret-string

```
For your GitHub ID and secret go on to your GitHub profile > developer settings and for Google you would have to go to the console google cloud

Next, create a ```component/SessionWrapper.js ``` and add the below code

```bash

"use client"
import { SessionProvider } from "next-auth/react";

const SessionWrapper = ({children}) => {
  return (
    <SessionProvider>{children}</SessionProvider>
  )
}

export default SessionWrapper
```

Also, in ```layout.js``` add 

```import SessionWrapper from "./component/SessionWrapper";```

and 

wrap body in ```<SessionWrapper>//body///</ SessionWrapper>``` 








