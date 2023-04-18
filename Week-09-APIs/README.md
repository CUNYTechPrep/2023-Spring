# How to use APIs effectively. 

## Agenda
1. [Lecture](https://docs.google.com/presentation/d/1ZC_nOOJxsBlizB8HkdSKg6d3JyhvgqETvXGXlFnjzs4/edit#slide=id.p)
2. Live coding
3. Setup
4. Breakout Rooms
5. Review what yall made. 

## Getting started with ChatGPT's API
1. Signup to use their api [here](https://platform.openai.com/signup)
1. We will be following their [getting started guide](https://platform.openai.com/docs/quickstart/build-your-application)
1. Scroll down to step #4 and choose which language you are going to use, NodeJS or Python.
1. Clone their getting started repo.
    * For JavaScript Node: `git clone https://github.com/openai/openai-quickstart-node.git`
    * For Python Flask: `git clone git clone https://github.com/openai/openai-quickstart-python.git`
1. Generate and save your API credientials into a temp text file. ([#4 in their getting started guide](https://platform.openai.com/docs/quickstart/build-your-application)).  
    * Click the 'create new secret key' and copy that key into a safe place for use later. 
1. Navigate to inside the repo you just cloned.
    * `cd openai-quickstart-python`
    * or `cd openai-quickstart-node`
1. Copy the `.env.example` file into a new file `.env`
1. Open `.env`:
    * On mac `nano .env`.
    * On windows open `.env` with your text editor.
1. In that file, set the `OPENAI_API_KEY` equal to your secret API key.
