# The Story Behind FlowHub

## Why This Project Exists

This project is a **living example**—not a demo app. I want to build a **real, production-ready application**. If you have the infrastructure set up, you can use this in production. That's the level of readiness I'm aiming for.

I'm a QA Automation Engineer. I don't know TypeScript deeply—I can read code and understand it a little bit, but I don't exactly know how to write TypeScript code. But my purpose, my agenda, is this: when I do testing, I often feel like, "Why are the developers not able to do this? Why are they not doing it?" I want to understand what goes into production. When you're developing a feature from PRD to production, what happens? What does the developer think? That thinking—I always want to have that.

So I thought: I need to do automation. QA automation. Will I have Playwright automation? Will I have to do API testing? When I want to test at a level, because I'm currently learning things, I don't have the setup ready. There's no proper setup. Even if I want to do API testing, there are no available tools to do testing. That's the biggest concern. And that's where this project started. That's the main purpose why this project started.

## The Turning Point: Accessibility Over Structure

Initially, I didn't start this project. I just made a small website and kept it on Render and all these things, and I just had different pages. This all started at one place: when I was reading HTML elements, I always had this question—how do we write XPaths, how do we write CSS selectors so strongly that even if tomorrow the UI changes, we don't break?

But I understood at a core level that **that is not going to work**. The point I understood is this: Selenium or some other tools focus on the DOM structure, while Playwright is a tool where they exactly focus on the **accessibility of the browser page**. 

Accessibility is something where we allow users—it gives semantic meaning, not just structural things. It gives semantic meaning to a button or a layout, anything. It gives us that so that even non-sighted people, all these people, can also use the website with screen readers. And that's where my turning point started. That's where I started: I need to test different elements, how exactly the accessibility will be there. I want to test it, but there is no infrastructure ready for me to test it. Then I started to develop myself so that I can test it. When I started it, it went well.

## From Random Pages to FlowHub

But after that, what happened is that I felt like we can make it more versatile. We can make it even something, not just some random pages like that. Then I had a conversation with GPT and understood what can I do. Then he proposed me a plan called **FlowHub**. 

He told me: this is not just a simple demo app. It contains a login, it contains the crucial things—where you can manage things, where you can update things, you can manage your timeline, all these things, the work you are doing. And it has an admin panel, you can do all the CRUD operations. And he proposed me this FlowHub project, where you can do almost all the things that people will do. You can add some complexity. And that's where I really loved the idea. Okay, why can't we just develop that? Because I don't know how to code. So I'll use AI to code and do everything. I'll focus on the testing part.

## What I'm Learning: The Complete SDLC

Now, what I'm focusing on, where I'm really interested in this project, is this: I want to understand the software development lifecycle. What happens? How things are like—how do PRD documents come? How do business requirement documents come? How do we get the functional requirements? From there, how do we extract the system-level design? How do we set up hosting? How do we run it?

I want to set up unit testing—where we mock the software. I want to do mock testing also, where we don't actually call the backend servers, we'll mock it. And then I want to do integration testing, where the hook layer, whatever the hook layer will be there, the core logic in the frontend. I want to test the hooks and with the integration layer. And then I'll be doing API testing in the black box, with requests to the module. And then I'll be doing Playwright with Python, in the black box testing mode only. And I'll be having sanity testing, smoke builds, so that we'll set up for GitHub Actions. So whenever we push the code, it automatically happens.

So I get a complete handover of what I can test. For example, if tomorrow I want to make it a little complex, I can use this project and make it a little complex, and I can test it in real time and understand what are the difficulties in testing like this feature. So then I can get more hands-on, and that way this project became more and more personal.

## The Journey: Iterations and Learning

I think I have already spent so many days, so many hours into it, and I have learned so many things. Initially, I started with no knowledge, and now like every day is a challenge. So now, see, this is the third iteration or fourth iteration we are doing. Again, we are starting from fresh. So every time I am learning something new and understanding the things. So this is the intent behind the FlowHub project.

## The Three Versions Strategy

The point is like we will have three versions:
- **Version 1** will be organization-focused. We will have the same kind of setup like all the organizations inside the UI, where we use Bootstrap.
- **Version 2** will be startup-focused, where we use Tailwind in a startup-focused UI.
- **Version 3** will be automation-focused, using custom CSS. Like where we make the automation a little challenging.

So it is not the same how it works in version 1 and version 3. In version 3, we intentionally make the automation really complex. So where we introduce shadow DOMs, where we introduce iframes, and where we make it a little challenging. We don't want the user to predict every day how the flow will be. We want to keep it a little challenging so that even if tomorrow there are animations introduced, there are so much of things—the predictability is a little low. At that time, we want to challenge the person who is automating: how do they handle all these complex scenarios?

## The Selector Philosophy

When it comes to selectors, we will go with data-testid, aria-label, aria-labelledby, and sr-only (visually hidden text). By these three ways, they can get the name, right? When they use aria-label, they have to update everything when they manually change it, and there is a downfall if they do not update their memory contents. That is where we can use aria-labelledby. The aria-labelledby is something where you refer to the recent element whatever you have, so that when it changes, automatically the aria-labelledby will change. And the third part is like where we put a short pixel which is invisible to the UI, the backend image, like where the sr-only, where you cannot directly control it.

Data-testid and aria-labels, we will use the semantic meaning. The semantic meaning—CSS and XPath are the last option for us. That is not the first resort, that is the last for us.

## Why This Matters: A Contribution to the QA Community

I want this project to be a proposal so that if tomorrow someone wants to automate something, instead of automating some random sites, they can just take this project and they can automate it and they can showcase there. That is the main intent I was thinking, the better use case.

I want to make this project open source. I want to make it to a QA community, so where people can come and expose their skills. Instead of just doing something, they can come and they can do automation, they can explore things. And this is my contribution to the QA community.

Today the point is that we may not be in a position to handle all the requests. But what we can do is that we can develop the code and put the instructions very clearly. If someone wants to do testing, they can set up all the things, whatever we have done. They will just take the repo, they will just do the setup, and they can do testing. So they can set up all these things in their local machines and they can do all the testing, whatever they want. It's just the docker images. They can just set up the two docker images and they can do the testing, whatever they want. So they can run the frontend and backend and all the things they can get there. Because this is a contribution from our side to the QA community.

## Local vs Production: Making It Accessible

When we are testing locally, we'll mock S3, Redis, Email. We'll keep it minimized, that we'll do. Because we cannot expect everybody to have the dependency. So we'll mock everything when we are testing in local. When we test it in production, we'll use the real things. For example, if people want to deploy it on cloud, we'll set up everything ready. They can just take it and follow the instructions, and they can set it up. Or else if they don't want to do, it's up to the people how they want to use it.

## The Testing Stack

For testing, we'll use Python and Playwright. Python and Playwright we'll use. And for API testing, we'll use Python API, like request library, and PyTest we'll use. Postman, we don't focus on Postman collection, because I try to do it now, because we don't really have the control. It's up to people how they want to do it.

## The Goal: No Looking Back

The reason why I'm sharing all these stories is like, you get the basic understanding of what is happening, what is the story behind it, and why we are making it, so that we make a meaningful conversation and this time there is no looking back. Every step what we take, we take it with little care and we do it in a proper way. We develop some feature, we do the unit testing, integration testing, the backend testing, the API testing, and then we will do the Playwright automation, everything. So we will incrementally increase the automation testing along with the development, and testing will go hand in hand. That is how we will plan and that is how we will do.

## My Personal Investment

I think I have already spent more than like so many days, so many days and so many hours into it, and I have learned so many things. Initially, I started with no knowledge, and now like every day is a challenge. This is the third iteration or fourth iteration we are doing. Again, we are starting from fresh. So every time I am learning something new and understanding the things.

The reason why I shared all these things is like, I want you to understand the intent why I am making and how much love and power I am putting into it. And I want to be correct. Tomorrow after doing half of the work, I should not get a feeling, okay, I really tried to prevent this, okay, then again I have to start fresh again. And tomorrow see, I want to think about, there might be scenarios which I was, I do not even think or thought about, I do not even aware of, and that is where it will become a challenge if I have known after doing so much of work. If tomorrow somebody can come and ask me something, it is like, you know, I may not be in a situation to answer them also. So the reason why I am sharing all these stories is like, you get the basic understanding of what is happening, what is the story behind it, and why we are making it, so that we make a meaningful conversation and this time there is no looking back.

---

**This is not just a project. This is my journey to understand software development from the inside out, and my contribution to the QA community.**

