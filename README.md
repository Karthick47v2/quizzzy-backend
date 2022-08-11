<h1 align="center">Welcome to quizzzy-backend 👋</h1>
<p>
  <a href="#" target="_blank">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-yellow.svg" />
  </a>
  <a href="https://codecov.io/gh/Karthick47v2/quizzzy-backend" > 
    <img src="https://codecov.io/gh/Karthick47v2/quizzzy-backend/branch/main/graph/badge.svg?token=YfdXXboTdc"/> 
 </a>
</p>

> This is backend for [Quizzzy](https://github.com/Karthick47v2/quizzzy/), Python application which automatically generate **_MCQ (specially factoid)_** questions and answers from given context, uses FastAPI to build APIs.

Automatically generating question from context is a trival task till today. This approach uses transformers and other algorithms to generate both question and answers from given context. We can divide the problem (Automatic question & answer generation) into many subproblems.

> NOTE: This is applicable only to MCQ question generation.

Workflow is,

1. Answer generation
2. Question generation
3. False answers generation aka Distractors

- ### Answer generation

Answer will always lies in given context. So, we just need to extract the answers from the context. To extract answers aka keyword from context, [keyBERT](https://github.com/MaartenGr/KeyBERT) is used, which is a powerful and easy-to-use keyword extraction technique. These extracted keywords are the answers for the questions which is going to be generated.

- ### Question generation

T5 transformers are used to generate questions when answer and context are given. These transformer were finetuned on `SQuAD` (reading comprehension QA) and `SciQ` (Science exam QA) datasets. Training details are in this [repo](https://github.com/Karthick47v2/question-generator). Those transformers will generate reading comprehensive questions and logical questions.

- ### False answers generation

False answers needs to be in same context as correct answer and needs to distract user. [sense2vec](https://github.com/explosion/sense2vec) library and other custom algorithms are used to tackle this problem. But, still, word sense disambiguation is a challenging task, sometimes output won't be desireable. So, Human-In-The-Loop method is used here. Which is, users can modify false answers if generated one is not desireable. This is implemented in [frontend](https://github.com/Karthick47v2/quizzzy/).

## Prerequisite

- Docker
- Firebase Firestore

## Usage

> Build Docker image by cloning the repo.

```sh
$ git clone https://github.com/Karthick47v2/quizzzy-backend.git
$ docker build -t quizzzy/latest .
```

> Run container from image.

```sh
$ docker run --name mcq-gen -d -p 8080:8080 quizzzy/latest
```

> Now it will be accessible on http://localhost:8080

> Now you can generate questions and answers by sending POST request to http://localhost:8080/get-questions/

Body of POST req should be,

```sh
{
    context: <context needed to generate questions and answers>
    uid: <firebase user unique id >
    name: <title for questionnaire>
}
# flask app designed in a way to automatically send generated ans and question to requested flutter app user's auth id
```

## Run tests

```sh
pytest app/tests/
```

> You can also view the code coverage report [here](https://app.codecov.io/gh/Karthick47v2/quizzzy-backend)

## Author

👤 **Karthick T. Sharma**

- Github: [@Karthick47v2](https://github.com/Karthick47v2)
- LinkedIn: [@Karthick47](https://linkedin.com/in/Karthick47)

## 🤝 Contributing

Contributions, issues and feature requests are welcome!<br />Feel free to check [issues page](https://github.com/Karthick47v2/quizzzy-backend/issues).

## Show your support

Give a ⭐️ if this project helped you!
