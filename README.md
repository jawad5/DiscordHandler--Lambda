# DiscordHandler--Lambda
Here's a detailed `README.md` file for your setup:

```markdown
# Discord Auction System - Lambda Function

This project is designed to handle the auction flow between Shopify and Discord using AWS Lambda, SQS, DynamoDB, and EC2. The system listens for incoming messages from Shopify, triggers auctions on Discord, and handles order processing and multilingual support.

## Project Structure

```
├── src
│   ├── discordClient.ts         # Logic to interface with Discord
│   ├── index.ts                 # Main Lambda function logic
│   ├── messageTemplates.json    # Multilingual message templates
├── test
│   ├── handler.test.ts          # Unit tests for the Lambda function
├── package.json                 # Node.js dependencies
├── tsconfig.json                # TypeScript configuration
└── README.md                    # Project documentation
```

### Key Components

1. **Lambda Function (`handler.ts`)**: This function processes auction events from Shopify, sends Discord messages using `discordClient`, and updates auction status in DynamoDB.
   
2. **Discord Client (`discordClient.ts`)**: This file contains logic for interacting with Discord using the `discord.js` library to send messages and manage auctions.

3. **Message Templates (`messageTemplates.json`)**: A file that stores multilingual message templates (e.g., English and German) for various stages of the auction and ticket processing.

4. **DynamoDB**: Stores the auction state and relevant data.

5. **SQS**: Acts as the intermediary between Shopify and Lambda, triggering the Lambda function on new messages.

6. **EC2**: Handles the pre-run scripts and Discord connection to ensure real-time communication.

## Setup Instructions

### Prerequisites

- Node.js (v16+)
- AWS CLI configured with appropriate credentials
- AWS Lambda, DynamoDB, and SQS setup on your AWS account
- TypeScript compiler (`tsc`)

### Installation

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/SneakerManager/DiscordHandler--Lambda.git
   cd DiscordHandler--Lambda.git
   ```

2. **Install Dependencies:**
   Install the required Node.js packages:
   ```bash
   npm install
   ```

3. **Configure AWS SDK**:
   Ensure that you have AWS credentials configured using `aws configure` or via environment variables.

4. **Configure Discord Bot**:
   Update your `discordClient.ts` file with your bot token and other relevant Discord configurations.

### Configuration

Ensure you have a `tsconfig.json` with the following options:

```json
{
  "compilerOptions": {
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "module": "commonjs",
    "moduleResolution": "node",
    "strict": true,
    "target": "ES2020"
  }
}
```

### Running the Project

#### 1. Local Development

For local development, you can use AWS SAM (Serverless Application Model) to simulate Lambda function calls and interactions.

- **Compile TypeScript:**
   ```bash
   npm run build
   ```

- **Run Locally:**
   Use SAM CLI or AWS Lambda mock to run locally:
   ```bash
   sam local invoke
   ```

#### 2. Deploying to AWS

To deploy the Lambda function to AWS, package the code and deploy it using AWS SAM or the AWS CLI.

- **Package the Function:**
   ```bash
   npm run build

   sam build
   ```

- **Deploy the Function:**
   ```bash
   sam deploy --guided
   ```

### Testing

Unit tests are written using Jest. To run the tests:

```bash
npm test
```

The tests are located in the `test/` directory, covering the main logic of the Lambda function, including Discord message handling and auction processing.

### Message Templates

The message templates are located in `messageTemplates.json`. You can easily add or modify templates here.

The format of the template is as follows:

```json
{
  "0500.Pending": {
    "EN": "Your auction is pending.",
    "DE": "Ihre Auktion ist ausstehend."
  },
  "1000.Auction": {
    "EN": "The auction has started!",
    "DE": "Die Auktion hat begonnen!"
  }
}
```

You can reference these templates in the Lambda function by their ID and language code:

```typescript
const message = generateMessage('0500.Pending', 'EN');
```

### Multi-Lingual Support

The system supports both English (`EN`) and German (`DE`). You can add more languages by updating the `messageTemplates.json` file and passing the appropriate language code when generating messages.

### Error Handling

The system includes error handling for the following cases:

- **Template Not Found**: If the `templateId` is not found in `messageTemplates.json`, an error will be thrown.
- **Unsupported Language**: If the message is not available in the requested language, an error will be thrown.

## Environment Variables

Set the following environment variables in your Lambda function configuration:

- `DISCORD_BOT_TOKEN`: The token for your Discord bot.
- `DYNAMODB_TABLE_NAME`: The name of your DynamoDB table for storing auction data.

You can set these variables in the AWS Lambda console or by defining them in the AWS SAM template.

## Future Improvements

- **Additional Language Support**: Easily extend the system to support more languages by adding templates.
- **Scalability**: Optimize the EC2 instance to scale based on demand for better cost efficiency.
- **Improved Error Reporting**: Enhance error logging and reporting for better debugging.


### Key Sections Explained:

- **Project Structure**: Provides an overview of the files and their roles.
- **Setup Instructions**: Steps to install, configure, and run the project locally or in AWS.
- **Message Templates**: Explains how to use and extend the message template system.
- **Multi-Lingual Support**: Provides details on the multi-language feature and how to add new languages.
- **Future Improvements**: Outlines possible future enhancements.

This `README.md` will help anyone working on the project understand its purpose, setup, and usage.