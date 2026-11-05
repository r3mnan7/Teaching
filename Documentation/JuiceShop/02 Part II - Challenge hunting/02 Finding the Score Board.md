---
title: "Finding the Score Board"
source: https://pwning.owasp-juice.shop/companion-guide/latest/part2/score-board.html
tags: [juiceshop, owasp, companion-guide]
---

# Finding the Score Board

Part of [[01 Challenge hunting|Challenge hunting]]
← Previous: [[01 Challenge hunting|Challenge hunting]]
Next: [[03 Injection|Injection]] →

In part 1 you were introduced to the Score Board and learned how it tracks your challenge hacking progress. You also had a "happy path" tour through the Juice Shop application from the perspective of a regular customer without malicious intentions. But you never saw the Score Board, did you?

## Challenges covered in this chapter

| Name | Description | Difficulty |
|---|---|---|
| Score Board | Find the carefully hidden 'Score Board' page. | ⭐ |

## Find the carefully hidden 'Score Board' page

Why was the Score Board not visited during the "happy path" tour? Because there seemed to be no link anywhere in the application that would lead you there! You know that it must exist, which leaves two possible explanations:

1. You missed the link during the initial mapping of the application

2. There is a URL that leads to the Score Board but it is not hyperlinked to

- Try to find a reference or clue behind the scenes. Or simply guess what URL the Score Board might have.

- Guessing the URL is quite easy if you consider the structure of other frontend paths in the application.

[[01 Challenge hunting|Challenge hunting]] [[03 Injection|Injection]]

