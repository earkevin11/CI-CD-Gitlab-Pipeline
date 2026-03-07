# CI-CD-Gitlab-Pipeline

## This pipeline is all defined by a Gitlab YAML file.

- Once you have your directory and terraform files and yaml file set up your good to go.
<img width="342" height="286" alt="image" src="https://github.com/user-attachments/assets/746c159c-6857-4bed-8c8f-8e4df2805d50" />


## 1. Creating a new branch and commiting it. Never commit directly to the Main branch!
- When you make a change to the source code, you should create a new branch and commit so that it goes through the Merge Request pipeline.
<img width="1135" height="349" alt="image" src="https://github.com/user-attachments/assets/073d171c-acdf-4031-9b13-4f965962a89d" />


## 2. Create a Merge Request for your feature, which triggers the feature branch MR pipeline  

<img width="1248" height="581" alt="image" src="https://github.com/user-attachments/assets/7be60257-2176-4d37-b12c-8c3d1b6af885" />

## 3. Once the MR pipeline completes, click merge to the Main pipeline

<img width="1039" height="590" alt="image" src="https://github.com/user-attachments/assets/1593f20b-a023-45e5-9621-ada68f656b8d" />

## NOTE: Our gitlab yaml file will 2 pipelines:
1. Feature branch MR pipeline
2. Main pipeline

## MR Pipeline
Feature branch MR pipeline has 2 stages:
1. init — initialize
2. plan — preview changes and shows you what will be created, changed, and destroyed

## Main Pipeline
Main Pipeline has 5 stages:
1. init
2. plan
3. review (manual click)
4. apply — deploy (manual click)
5. destroy — tear down (manual click)


<img width="1231" height="534" alt="image" src="https://github.com/user-attachments/assets/7f6d25a4-69f3-4858-87d6-c24361378434" />


## Why init and plan run on the MR first?
When you open a Merge Request, GitLab triggers a pipeline on your feature branch. The purpose of running init and plan at this stage is so that you and your team can review what Terraform is going to change BEFORE it gets merged to main.

Think of it like a preview:
feature branch MR pipeline
├── init  ← sets up Terraform, confirms backend connects
└── plan  ← shows exactly what will be created/changed/destroyed

You can click into the plan job logs and see output like:
+ azurerm_resource_group.rg will be created
+ azurerm_virtual_network.vnet will be created
~ azurerm_subnet.subnet will be updated

## IMPORTANT: This lets you catch mistakes before they ever touch real infrastructure.

<img width="723" height="500" alt="image" src="https://github.com/user-attachments/assets/15575e08-ebd9-43df-ba4b-6d4e3c39c2c6" />



# Frequently asked questions?

## Why does plan run twice?
- MR Plan - Purpose is code review
- Main Plan - For infrastructure approval

## What could happen if you skip MR plan?
- Skipping the MR plan would mean merging blind into Main branch without knowing the impact.
- Skipping the main plan would mean applying a potentially stale plan. Both serve different purposes and both are needed.

## Is it best practice to have terraform plan in both a Merge Request pipeline and a Main pipeline?
- Yes, this is intentional and is actually best practice because it catches issues before it reaches Main pipeline.
- It is a secondary check before Terraform apply is ran in Main pipeline!
- In short — the MR init and plan are your safety check before merging. They answer the question "is this safe to merge?" before anything touches main or real infrastructure.

<img width="728" height="1098" alt="image" src="https://github.com/user-attachments/assets/005bd402-81cb-4b83-ab86-bf4aa9d8a654" />
