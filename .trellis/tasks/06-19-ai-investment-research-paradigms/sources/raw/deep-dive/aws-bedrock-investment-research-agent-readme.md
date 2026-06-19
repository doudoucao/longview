[Skip to content](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#start-of-content)

You signed in with another tab or window. [Reload](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md) to refresh your session.You signed out in another tab or window. [Reload](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md) to refresh your session.You switched accounts on another tab or window. [Reload](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md) to refresh your session.Dismiss alert

{{ message }}

[awslabs](https://github.com/awslabs)/ **[amazon-bedrock-agent-samples](https://github.com/awslabs/amazon-bedrock-agent-samples)** Public

- [Notifications](https://github.com/login?return_to=%2Fawslabs%2Famazon-bedrock-agent-samples) You must be signed in to change notification settings
- [Fork\\
273](https://github.com/login?return_to=%2Fawslabs%2Famazon-bedrock-agent-samples)
- [Star\\
798](https://github.com/login?return_to=%2Fawslabs%2Famazon-bedrock-agent-samples)


## Collapse file tree

## Files

main

Search this repository(forward slash)` forward slash/`

/

# README.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

[![LucaiB](https://avatars.githubusercontent.com/u/76792861?v=4&size=40)](https://github.com/LucaiB)[LucaiB](https://github.com/awslabs/amazon-bedrock-agent-samples/commits?author=LucaiB)

[InvestmentResearchAgent - Update (](https://github.com/awslabs/amazon-bedrock-agent-samples/commit/9aa94fb43f73e0468ba7c16d75beb766d5f5b1b4) [#105](https://github.com/awslabs/amazon-bedrock-agent-samples/pull/105) [)](https://github.com/awslabs/amazon-bedrock-agent-samples/commit/9aa94fb43f73e0468ba7c16d75beb766d5f5b1b4)

Open commit details

last yearApr 9, 2025

[9aa94fb](https://github.com/awslabs/amazon-bedrock-agent-samples/commit/9aa94fb43f73e0468ba7c16d75beb766d5f5b1b4) · last yearApr 9, 2025

## History

[History](https://github.com/awslabs/amazon-bedrock-agent-samples/commits/main/examples/multi_agent_collaboration/investment_research_agent/README.md)

Open commit details

[View commit history for this file.](https://github.com/awslabs/amazon-bedrock-agent-samples/commits/main/examples/multi_agent_collaboration/investment_research_agent/README.md) History

110 lines (86 loc) · 3.25 KB

/

# README.md

Copy path

Top

## File metadata and controls

- Preview

- Code

- Blame


110 lines (86 loc) · 3.25 KB

[Raw](https://github.com/awslabs/amazon-bedrock-agent-samples/raw/refs/heads/main/examples/multi_agent_collaboration/investment_research_agent/README.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# Investment Research Assistant Agent

[Permalink: Investment Research Assistant Agent](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#investment-research-assistant-agent)

Investment Research supervisor agent has three collaborators, a News agent, a Quantitative Analysis Data agent, and a Smart Summarizer agent. These specialists are orchestrated to perform investment analysis for a given stock ticker based on the latest news and recent stock price movements.

Note

**Results from these agents should not be taken as financial advice.**

## Architecture Diagram

[Permalink: Architecture Diagram](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#architecture-diagram)

[![architecture](https://github.com/awslabs/amazon-bedrock-agent-samples/raw/main/examples/multi_agent_collaboration/investment_research_agent/architecture.jpg)](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/architecture.jpg)

## Prerequisites

[Permalink: Prerequisites](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#prerequisites)

1. Open a terminal then clone and install repository, setup environment

```
git clone https://github.com/awslabs/amazon-bedrock-agent-samples

cd amazon-bedrock-agent-samples

python3 -m venv .venv

source .venv/bin/activate

pip3 install -r src/requirements.txt
```

2. Deploy Web Search tool. You will be required to make a [Tavilly API](https://docs.tavily.com/docs/gpt-researcher/getting-started) key.

Follow instructions [here](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/src/shared/web_search).

3. Deploy Stock Data Lookup and Portfolio Optimization tool. This stack may take ~5 minutes to deploy.

Follow instructions [here](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/src/shared/stock_data).

### For [main.ipynb](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/main.ipynb)

[Permalink: For main.ipynb](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#for-mainipynb)

Run through the cells in the notebook, attach appropriate permissions as needed. For this notebook, you may attach the following IAM policy to the appropriate execution role to run through the notebook:

Important

This IAM policy is highly permissive and grants access to multiple critical AWS services including IAM, S3, Lambda and Bedrock.
It should only be used for quick prototyping or development in isolated, non-production environments

```
{
    "Version": "2012-10-17",
    "Statement": [\
        {\
            "Sid": "AllowBedrockScopedAccess",\
            "Effect": "Allow",\
            "Action": [\
                "bedrock:*"\
            ],\
            "Resource": "*"\
        },\
        {\
            "Sid": "AllowS3ReadWriteLimited",\
            "Effect": "Allow",\
            "Action": [\
                "s3:GetObject",\
                "s3:PutObject",\
                "s3:ListBucket",\
                "s3:CreateBucket"\
            ],\
            "Resource": [\
                "arn:aws:s3:::multimodal-fsi-data*",\
                "arn:aws:s3:::bda-processing*/*"\
            ]\
        },\
        {\
            "Sid": "AllowAOSSReadWrite",\
            "Effect": "Allow",\
            "Action": [\
                "aoss:*"\
            ],\
            "Resource": "*"\
        },\
        {\
            "Sid": "AllowIAMMinimal",\
            "Effect": "Allow",\
            "Action": [\
                "iam:GetRole",\
                "iam:ListRoles",\
                "iam:PassRole"\
            ],\
            "Resource": [\
                "arn:aws:iam::*:role/*AmazonBedrock*"\
            ]\
        },\
        {\
            "Sid": "AllowLambdaInvokeOnly",\
            "Effect": "Allow",\
            "Action": [\
                "lambda:InvokeFunction",\
                "lambda:GetFunction",\
                "lambda:ListFunctions"\
            ],\
            "Resource": "arn:aws:lambda:*::function:*"\
        }\
    ]
}
```

For more information on creating IAM policies, check out our [documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html)

## License

[Permalink: License](https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md#license)

This project is licensed under the Apache-2.0 License.

You can’t perform that action at this time.