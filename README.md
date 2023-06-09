# ChatGPT Remix Vercel

This is a Remix Stack focused on creating a ChatGPT conversations interface, it runs on Vercel serverless functions, with Vercel Storage. It supports multiple users, response streaming, multiple conversations, and persistent message history. It was based off the [Remix Indie Stack](https://github.com/remix-run/indie-stack), so it is has everything you need to get you started exploring.

Learn more about [Remix Stacks](https://remix.run/stacks).

## Motivation

I created this project because I could not find complete examples on how to use both Remix and the ChatGPT API with streaming and persistent conversation history, without getting overly complex. The idea is for this to be something anyone can run on their machine and deploy Vercel, without needing any third-party services. I know a lot of people want to learn about this topic, so I hope this helps some of you get up and running faster.

This will be an evolving project to which I will add new features, better styling, etc. So if you're interested, hop on for the ride!

## Features

- Multiple users
- Multiple conversations per user
- Message history stored in the database
- Context-aware conversations based on all previous messages up to the token limit for the model
- System message (allows instructions for assistant personalization)
- Message streaming ("responds as if typing") via SSE (Server-side-events)

## What's in the stack

- Integration with [OpenAI's Chat completions API](https://platform.openai.com/docs/guides/chat)
- Database [PostgreSQL Database](https://www.postgresql.org/)
- Email/Password Authentication with [cookie-based sessions](https://remix.run/utils/sessions#md-createcookiesessionstorage)
- Database ORM with [Prisma](https://prisma.io)
- Styling with [Tailwind](https://tailwindcss.com/)
- Code formatting with [Prettier](https://prettier.io)
- Static Types with [TypeScript](https://typescriptlang.org)

There's a lot more included (cypress, vitest, eslint, etc.) from the original stack, but I don't mention it above because I haven't configured them for this project yet. See the roadmap.

## Screenshots

Here are a few screenshots of what it looks like right now:

<p float="left">
  <img src="https://perezcarreno.com/wp-content/uploads/2023/03/chatgpt-remix-1.png" width="45%" />
  <img src="https://perezcarreno.com/wp-content/uploads/2023/03/chatgpt-remix-2.png" width="45%" />
  <img src="https://perezcarreno.com/wp-content/uploads/2023/03/chatgpt-remix-3.png" width="45%" />
  <img src="https://perezcarreno.com/wp-content/uploads/2023/03/chatgpt-remix-4.png" width="45%" />
</p>

## Development

- Get a free API key at [OpenAI](https://platform.openai.com/account/api-keys)

- Install dependencies

  ```sh
  npm install
  ```

- Initialize the project

  ```sh
  npx remix init
  ```

- Initialize git and push to a new Github repository (or create a fork)

- Create a new Vercel project by importing your new repository.

- Add your own OpenAI API Key and a Session Secret to your Vercel Environment variables when deploying for the first time.

  ```sh
  SESSION_SECRET="super-duper-s3cret"
  OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXX
  ```

- Go to the Storage tab in your Vercel project, and add PostgreSQL Storage (new feature).

- In your Vercel project settings, go to "Environment Variables" and create a new variable called 'POSTGRES_PRISMA_URL_NO_TIMEOUT'. Copy the contents of 'POSTGRES_PRISMA_URL' and remove the ending '&connect_timeout=15'. For some reason, it doesn't work with it at the moment.

- Copy .env.example to .env and add your environment variables from your Vercel project.

  ```sh
  SESSION_SECRET="super-duper-s3cret"
  OPENAI_API_KEY=sk-XXXXXXXXXXXXXXXXXXXX
  POSTGRES_URL="XXXXXX"
  POSTGRES_URL_NON_POOLING="XXXXXX"
  POSTGRES_PRISMA_URL_NO_TIMEOUT="XXXXXX"
  POSTGRES_USER="XXXXXX"
  POSTGRES_HOST="XXXXXX"
  POSTGRES_PASSWORD="XXXXXX"
  POSTGRES_DATABASE="XXXXXX"
  ```

- Initial setup:

  ```sh
  npm run setup
  ```

- Start dev server:

  ```sh
  npm run dev
  ```

This starts your app in development mode, rebuilding assets on file changes.

The database seed script creates a new user with some data you can use to get started:

- Email: `test@account.com`
- Password: `1q2w3e4r`

### Relevant code:

This is a basic implementation of a ChatGPT conversations interface, but it's a good example of how you can build a full stack app with Prisma and Remix. The main functionality is creating users, logging in and out, and creating and deleting conversations that interact with the ChatGPT API. You can change the system message to have it follow your desired personality.

- creating users, and logging in and out [./app/models/user.server.ts](./app/models/user.server.ts)
- user sessions, and verifying them [./app/session.server.ts](./app/session.server.ts)
- creating, and deleting conversations [./app/models/conversation.server.ts](./app/models/conversation.server.ts)
- creating, and deleting messages [./app/models/message.server.ts](./app/models/message.server.ts)
- show messages inside a conversation and create new messages [./app/conversations/conversationId.tsx](./app/conversations/conversationId.tsx)
- interact with the ChatGPT API [./app/routes/completion.tsx](./app/routes/completion.tsx)

## Deployment

- Push to your main Github branch. Vercel should deploy automatically.

## Roadmap

- [x] Integrate streaming responses from the ChatGPT API
- [x] Keep persistent message history for each user and conversation in the database
- [x] Support for Vercel PostgreSQL Storage
- [ ] Step-by-step README
- [ ] Styling enhancements
- [ ] Mobile drawer sidebar
- [ ] Support for GPT4
- [ ] Authentication via Clerk
- [ ] System message UI
- [ ] Support for embeddings
- [ ] Support for other authentication methods
- [ ] Include unit tests

### Type Checking

This project uses TypeScript. It's recommended to get TypeScript set up for your editor to get a really great in-editor experience with type checking and auto-complete. To run type checking across the whole project, run `npm run typecheck`.

### Formatting

We use [Prettier](https://prettier.io/) for auto-formatting in this project. It's recommended to install an editor plugin (like the [VSCode Prettier plugin](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)) to get auto-formatting on save. There's also a `npm run format` script you can run to format all files in the project.

## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

## Contact

Armando J. Perez-Carreno - [@perezcarreno](https://twitter.com/perezcarreno)

Project Link: [https://github.com/perezcarreno/chatgpt-remix](https://github.com/perezcarreno/chatgpt-remix)

## Acknowledgments

A huge shoutout to these wonderful people and teams, without which none of this could be possible.

- [Remix](https://github.com/remix-run)
- [OpenAI](https://github.com/openai)
- [Vercel](https://vercel.com)
- [Joel (waylaidwanderer)](https://github.com/waylaidwanderer/node-chatgpt-api)
- [Travis Fischer](https://github.com/transitive-bullshit)
- [Sergio Xalambri](https://github.com/sergiodxa)
