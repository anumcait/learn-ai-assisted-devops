# How AI Assists a Devops Engineer

This repository contains tools and scripts that leverage generative AI (GenAI) to assist DevOps engineers in automating and optimizing various tasks, including system health monitoring and Dockerfile generation. These tools simplify complex tasks, improve productivity, and implement best practices using AI models like ChatGPT, Ollama, and Gemini.

In the context of DevOps engineering, generative AI can significantly enhance automation, productivity, and decision-making. Here's how it helps DevOps engineers:
Generative AI helps DevOps engineers by automating repetitive tasks, optimizing workflows, enhancing system reliability, and boosting productivity. By reducing manual efforts in coding, testing, monitoring, and infrastructure management, generative AI allows DevOps teams to focus more on strategy and problem-solving while maintaining high levels of efficiency and system performance.

# Prompt Engineering

When a user gives input to get the proper output, that input is called as prompt and in which way user gives input is the engineering.

There are different ways to generate output from the LLMs, they are

Zero shot - it is a general way giving input, without any special conditions or characteristics

Few shot - it is a way where user will give example input and ask for the same structure of the output

Multi shot - it is same like Few shot but in a lengthy way of examples or formats

Cot: Chain of thoughts - it is more advanced input


# Devops engineer wise enough to utilise the prompt engineering

# 🐳 Dockerfile Generator

A GenAI powered tool that generates optimized Dockerfiles based on programming language input. This project uses Ollama with the Llama3 model to create Dockerfiles following best practices.

## 📋 Prerequisites

### Installing Ollama

1. **Download and Install Ollama**
   ```bash
   # For Linux
   curl -fsSL https://ollama.com/install.sh | sh

   # For MacOS
   brew install ollama
   ```

2. **Start Ollama Service**
   ```bash
   ollama serve
   ```

3. **Pull Llama3 Model**
   ```bash
   ollama pull llama3.2:1b
   ```
   
#### Purpose
This Python tool uses GenAI models to generate optimized Dockerfiles based on the programming language specified by the user. It incorporates best practices such as multi-stage builds, using the right base images, and installing dependencies effectively.

## 🚀 Project Setup

1. **Create Virtual Environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Linux/MacOS
   # or
   .\venv\Scripts\activate  # On Windows
   ```

2. **Install Dependencies**
   ```bash
   pip3 install ollama
   ```
3. **create the Application**
   vi generate_dockerfile.py
   
5. **Run the Application**
   ```bash
   python3 generate_dockerfile.py
   ```
#### Code for Dockerfile Generation: `generate_dockerfile.py`
```
import ollama

PROMPT = """
ONLY Generate an ideal Dockerfile for {language} with best practices. Do not provide any description
Include:
- Base image
- Installing dependencies
- Setting working directory
- Adding source code
- Running the application
- Need multi stage docker build
"""

def generate_dockerfile(language):
    response = ollama.chat(model='llama3.2:1b', messages=[{'role': 'user', 'content': PROMPT.format(language=language)}])
    return response['message']['content']

if __name__ == '__main__':
    language = input("Enter the programming language: ")
    dockerfile = generate_dockerfile(language)
    print("\nGenerated Dockerfile:\n")
    print(dockerfile)
```
python3 generate_dockerfile.py

## Example Output:
If the user enters "node.js" as the language, the generated Dockerfile might look like this:
```
dockerfile

- Use the official Node.js 14 image as the base image
FROM node:14

- Install dependencies using npm
RUN npm install -g @types/node @babel/preset-env

- Set working directory to /app
WORKDIR /app

- Copy source code into the container at /app
COPY . .

- Run command to start a new server for development
CMD ["node", "server.js"]
```
## 💡 How It Works

1. The script takes a programming language as input (e.g., Python, Node.js, Java)
2. Connects to the Ollama API running locally
3. Generates an optimized Dockerfile with best practices for the specified language
4. Returns the Dockerfile content with explanatory comments

# 3. Hosted LLMs - Gemini Dockerfile Generator
Code for Hosted LLMs Dockerfile Generation: generate-hostedllm-gemini.py
```
import google.generativeai as genai
import os

# Set your API key here
os.environ["GOOGLE_API_KEY"] = "YOUR_GOOGLE_API_KEY"

# Configure the Gemini model
genai.configure(api_key=os.getenv("GOOGLE_API_KEY"))
model = genai.GenerativeModel('gemini-1.5-pro')

PROMPT = """
Generate an ideal Dockerfile for {language} with best practices. Just share the dockerfile without any explanation between two lines to make copying dockerfile easy.
Include:
- Base image
- Installing dependencies
- Setting working directory
- Adding source code
- Running the application
"""

def generate_dockerfile(language):
    response = model.generate_content(PROMPT.format(language=language))
    return response.text

if __name__ == '__main__':
    language = input("Enter the programming language: ")
    dockerfile = generate_dockerfile(language)
    print("\nGenerated Dockerfile:\n")
    print(dockerfile)
```
**OUTPUT**
python3 generate-hostedllm-gemini

vi generate-hostedllm-gemini.py

python3 generate-hostedllm-gemini.py

Enter the programming language: oracle

Generated Dockerfile:

```dockerfile
FROM oraclelinux:8-slim

RUN microdnf install -y oracle-epel-release-el8 && \
    microdnf install -y libaio bc && \
    microdnf clean all && \
    rm -rf /var/cache/yum

WORKDIR /opt/oracle

COPY --chown=oracle:oinstall database/ /opt/oracle/database/

USER oracle

COPY --chown=oracle:oinstall docker-entrypoint.sh /opt/oracle/

ENTRYPOINT ["/opt/oracle/docker-entrypoint.sh"]

EXPOSE 1521
```

## 🏆 Troubleshooting
- Make sure Ollama service is running before executing the script.
- Ensure the correct model is downloaded.
- Adapt best practices for other programming languages as needed.

