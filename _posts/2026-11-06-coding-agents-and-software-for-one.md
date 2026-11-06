---
title: "Coding Agents and Software for One"
---

This year, a family member of mine was treated for breast cancer. After surgery and chemotherapy, the last step was radiation therapy. Before it started, the oncologist explained how the sessions would work, and mentioned that she would need to use a technique called DIBH. It was one sentence in a long conversation, but it stayed with her.

DIBH asks the patient to do something: take a deep breath and hold it, in the right way, while the machine works. She did not know if she could do it well, and there was no way to practice before the first session.

So I made a [small app](https://rumple.github.io/dibh-coach/) for her to practice with. A coding agent did most of the work, and the first version took about an hour. It helped: on the day of her first treatment, she did the breath holds easily.

Nobody would build this app as a product. Too few people need it, and each of them needs it for only a few weeks. But when building takes an hour instead of days, an app like this becomes worth making for one person and one problem. **I think this is the part of coding agents that is not discussed enough: they make software for one person possible.** The rest of this post is the story of that app.

## How radiation therapy works

Radiation therapy uses beams of high-energy X-rays to kill cancer cells. The radiation damages the DNA inside cells. A cell with badly damaged DNA cannot divide, and it dies. Healthy cells are damaged too, but they are better at repairing the damage than cancer cells are.

This difference is the reason that the treatment is split into many small doses, called fractions. A typical course is one session each weekday for a few weeks. Between sessions, healthy cells have time to recover. Cancer cells recover less, so the damage to them adds up.

The machine is a linear accelerator. It rotates around the patient and aims beams at the tumor from different angles. Each beam passes through healthy tissue on the way in and on the way out, but the beams cross at the tumor, so the tumor gets the highest dose.

Before treatment starts, there is a planning session. A CT scan shows the exact position of the tumor and of the organs near it. The team uses the scan to plan the angles, the shape of each beam, and the dose. Small marks on the skin help the team put the patient in the same position every day. Each session is short. Most of the time goes into getting the position right, and the beam itself is on for only a few minutes.

**The plan is only correct if the body is in the same place as it was on the planning scan.** This is where breathing becomes a problem. Each breath moves the chest wall and the organs inside it. If a nearby organ moves into the path of the beam, it gets radiation that was meant for the tumor.

## What DIBH is

DIBH means deep inspiration breath hold. Each part of the name describes one step. "Inspiration" is the medical word for breathing in, and "deep inspiration" means a breath that fills the lungs much more than a normal breath. "Breath hold" means that you stop at the top of that breath and stay there while the beam is on. The other way to treat is called free breathing, where the patient breathes normally and the beam is aimed at a body that moves a little all the time.

In radiation therapy for the left breast, the heart is close to the area that is treated. Radiation to the heart can increase the risk of heart disease years later, so the team tries to keep the dose to the heart as low as possible. When you take a deep breath and hold it, your lungs fill, and your heart moves down and away from the chest wall. This puts more distance between the heart and the radiation beam.

The planning scan is also done with the breath held. So in every session, the patient must breathe in to the same depth as on the day of the scan. A breath that is too shallow puts the body in a different position from the plan.

During treatment, a machine watches the movement of the chest. The beam is on only when the breath is above a set level. If the breath drops below that level, the beam pauses. Each hold is about 20 seconds, and there are several holds in each session.

## The problem

Breathing sounds easy. But the first session was close, and she had questions that nobody could answer before the day:

- Can I hold my breath for 20 seconds while I lie still?
- Can I breathe in to the same level each time?
- What if my breath slowly drops during the hold, and the beam keeps stopping?

These questions made her anxious. She was already going through a tough time. She had been through a cancer diagnosis, surgery, and chemotherapy, and now radiation was about to start. That is a lot to carry, and now there was one more thing that she was not sure she could do. It was a small thing compared to the rest, but it was a thing that she would have to do herself, every day, with the machine waiting.

She wanted to practice. Practice gives confidence, and confidence was what she did not have. I looked for an app that she could use at home. I did not find one. There are general breathing apps, and there are clinical systems in hospitals. There was nothing in between: a way to practice the actual task, with feedback, at home.

## Building it

I used Claude Code. I described the problem, and in about one hour we had a first version that worked on her iPhone.

The idea is simple. The phone lies flat on the breastbone. When you take a deep breath, your chest rises and the phone tilts. The phone's motion sensor measures the tilt. That tilt is the breath signal.

The app works in three steps:

1. **Calibration.** You breathe calmly for 20 seconds. This sets 0%. Then you take three deep breaths and hold each one. This sets 100%.
2. **Practice.** For each hold, you breathe in until the level crosses a target line, for example 75%, and you hold it. While you are above the line, you hear a steady tone ("beam on"). If you drop below it, you hear double beeps ("beam paused"). A voice counts down the last seconds, and then you rest.
3. **Summary.** After the session, the app shows how much of each hold was above the line, how steady the breath was, and how many times it dropped.

The sounds are important. During treatment you lie on your back and you cannot look at a screen. So the feedback had to be something that she could hear with her eyes closed.

I did not write the code. I did not know how to read the iPhone's motion sensor from a web page, or how to filter the signal, or how to make Safari keep the screen awake. The agent did know. My part was to describe what she needed, try it, and say what was wrong.

After the first real session on her phone, I exported the session log and gave it back to the agent. It found problems that I had not seen. For example, the calm level drifted after calibration, and the calibration breaths were not as deep as the real holds. Over the next few days we made more changes: practice phases with different hold lengths, a check that the phone's sensor and sound work before a session, and screens that fit the phone. But the first useful version took one hour.

**It is a practice tool only.** It is not connected to any treatment machine, and its percentages are not the clinical threshold. The goal was not to copy the machine. The goal was to practice the skill: breathe in deeply, to the same level, and stay there.

## What happened

She practiced at home for several days. She learned how a good hold feels. She learned how deep to breathe so that she was well above the line, and how to stay still while she held it.

On the day of the first treatment, she did the breath holds easily. The oncologist said that it was one of the fastest sessions that they had had.

## What this says about coding agents

I could have built this app without an agent. But it would have taken me days, not an hour, and the treatment would have started before the app was ready. **When the time to build drops from days to an hour, a different kind of software becomes possible: software for one person, for one problem, for one week.**

This is why the app did not exist before. The market is small, the medical rules are many, and each patient needs it for only a few weeks. But for one person, at that time, it was exactly the right thing.

Most discussion is about how agents make professional programmers faster. That is true. But they also change *what is worth building*. A problem that is too small for a company, and too much work for a weekend, is now something that you can solve in an evening for someone you care about.

---

Links:

- App: [rumple.github.io/dibh-coach](https://rumple.github.io/dibh-coach/)
- Source code: [github.com/rumple/dibh-coach](https://github.com/rumple/dibh-coach)
