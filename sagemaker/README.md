# harpin AI Amazon SageMaker Algorithm

# Getting Started
## Subscribe to Amazon SageMaker Algorithm
1. Ensure you are logged in to the AWS account which has access to the data on which you want to run the harpin AI toolkit.
1. Visit the harpin AI ["Identity Resolution with AI and ML" listing](https://aws.amazon.com/marketplace/pp/prodview-etnavzupbnthk) on AWS Marketplace.
1. Click the "Continue to subscribe" button.
1. Review the offer details and click the "Continue to configuration" button.
1. On the "Configure and launch" screen, ensure the desired software version and AWS region are selected.
1. Copy the "Product Arn" to your clipboard as you will need it in subsequent steps.
1. Click the "View in Amazon SageMaker" button.

## Create a Jupyter Notebook in Amazon SageMaker
1. In the AWS Console, ensure you have selected the same AWS region that was selected above when subscribing to the SageMaker algorithm.
1. In Amazon SageMaker, click the "Notebooks" link in the left navigation panel.
1. From the "Notebook instances" tab, click the "Create notebook instance" button.
1. Complete the configuration as needed.  We suggest selecting at least an "ml.m5.xlarge" notebook instance type, but the actual instance type that is needed might be larger depending upon the amount of data being accessed.
1. Expand the "Git repositories" section, and select "Clone a public Git repository to this notebook instance only".
1. In the "Git Repository URL" field, enter the URL to this repository: `https://github.com/harpin-ai/toolkit-examples.git`.
1. Click the "Create notebook instance" button.
1. It will take a few minutes for the notebook to start.  Once it is started, click the "Open Jupyter" link to access the notebook.

## Running harpin AI Identity Resolution on Sample Data
1. Obtain the name of an S3 bucket that already exists or create a new bucket for use in this example.  Ensure the IAM role assigned to the SageMaker notebook has read and write access to the S3 bucket.
1. In the notebook that was launched via the previous step, open the `notebooks/Identity_Resolution_50k_Data_Sample.ipynb` notebook file.
1. In the first code block of the notebook, replace the string `YOUR S3 BUCKET` with the name of the S3 bucket you plan to use.
1. Also in the first code block of the notebook, replace the string `YOUR ALGORITHM ARN` with the algorithm ARN that AWS provided when you subscribed to the harpin AI AWS Marketplace offering.
1. Execute the code blocks in the notebook in order, observing the results of the resolution process.
