# CA-Student-Info-System
Screenshots of the student information system, for obvious reasons couldn't put up the actual codebase (but have included code samples of my work style at the time, good or bad), but have included some screenshots showing examples of my work and listing the functionality developed.

Used MSSQL Server 2015 (database), pojo Java/Hibernate (model), Spring Framework (data access/services/web controllers/security/etc), Aspects (audit-logging), jsps/jquery/jstl/bootstrap (frontend)

#### Side note - Just to give a little insight into my personality, here was the message Calgary Academy wrote on their weekly newsletter about my departure.
[Click here to open pdf](https://github.com/jaysait/CA-Student-Info-System/blob/master/_CAConnectionShowingDepartingMessage.pdf)

# Table of Contents
1. [Screenshots](#Screenshots)
2. [Code Samples](#Code_Samples)
3. [A little about me](#about)






## Screenshots

### Homepage
![homepage](https://github.com/jaysait/CA-Student-Info-System/blob/master/homepage.png)
- customizable calendar feed
- classes with ability for attendance, grades, rubrics, etc
- notifications about issues in the system ie. duplicate enrollments in subjects
- many reports

### Homepage 2
![homepage2](https://github.com/jaysait/CA-Student-Info-System/blob/master/homepage-2.png)
- quick lookups 
- parent teacher scheduling functionality
- online options allowing parents to pick options for the next year
- reenrollment functionality
- manage various aspects of the system

### Edit Profile
![Edit Profile](https://github.com/jaysait/CA-Student-Info-System/blob/master/edit-profile.png)
- used bootstrap for interface

### Yearend Recognition Awards
![Yearend Recognition Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards.png)
- used bootstrap for interface

### Yearend Recognition Awards - Customize Awards
![Yearend Recognition Awards - Customize Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-custom.png)

### Yearend Recognition Awards - Student Awards
![Yearend Recognition Awards - Student Awards](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-per-student.png)

### Yearend Recognition Awards - Export
![homepage](https://github.com/jaysait/CA-Student-Info-System/blob/master/yearend-recognition-awards-export.png)
- allowed graphic person to export awards and mail merge into the actual award documents

### Individual Program Plan
![Yearend Recognition Awards - Export](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-main.png)
- used jquery ui for frontend

#### Side note - because I loaded everything upfront, to pass the initial wait I would include random made up facts about individual program plans (IPP) to help pass the time, also shows a little bit about my personality/sense of humour
- One out of every 15 IPPs involve making better jerky
- In a small tribe in the Amazon, it's believed that IPPs are the work of sorcerers
- Raccoons are the only other documented animal that also practise IPPs...and as you'd expect most of the goals pertain to Algebra or opening cans
- the world's longest IPP measured 954 pages long...the irony...most were goals of getting to the point quicker
- In France IPPs are called PPIs...huh
- Did you know the first recorded IPP was in the Mesolithic age on rock...it said long term goal = do not get eaten by dinosaur...sadly without properly written objectives he was eaten
- Did you know the phrase IPP was an accident...he just had a stutter and had to go to the washroom...sadly he didn't get help for either
- 20% of IPPs from the 80s contain mustard stains on them
- During the war when paper/lead was sparse, IPPs were written on barns with honey
- The most common word used in IPPs...the, the second most common word...Cockamamie
- Countries that don't practise IPPs are 80% more likely to not know what a wheel is for but 20% more likely to own a cotton candy machine
- People from Romania, pronounce them 'YIPs' and people from Saskatchewan pronounce them 'dirty IPPys'
- 1 out of 4 teachers said that 25% of IPPs they write involve 'student X needs to stop sticking Y in Z so much'...sadly the success rate on those is only 30%
- IPPs are the 2nd most contributed tool of success for kids...the first one...tang or is it love
- The kid with a IPP with the record most goals achieved of 78 in 1 year has since had his record stripped due to the use of Human Growth Hormone.
- Help I'm stuck in a cupboard
- Calgary Academy staff are known worldwide for their great IPPs...that and consuming almost 2x the amount of alcohol of the average sailor
- 1 out of 3 teachers will lie on their IPPs to get a bigger bonus...so look to your left and right...one of you...shame
- Most people forget to do the M in SMART goals, that and their left sock...coincidence??
- Today only 2 for 1 deal on Huckleberry Pecan Marmalade, and tell them Lily sent you for a free melon baller
- Little known fact, the movie Titanic was based on a 7th grader's IPP in Delaware...or was it Weekend at Bernies...ahhh wikipedia...so factual
- If you put all the ipps in a row, it would go around the world 573 times...on a unrelated note...the rainforest is down  5%
- IPP goals come true 45% more often when a child puts it under their pillow and wishes it to come true or is it hard work/dedication, nope lets go with wishes
- If you're here working on ipps...WHOS WATCHING THE KIDS!!
- If any of your IPP goals involve 'pees' or 'poops' less...maybe IPPs shouldn't be a priority
- Have you been told lately you make such a difference, even if you don't always see it and look at you...did someone say teacher or runway model
- Without someone like you there wouldn't be someone like me...makes you kind of question your whole career path don't it 
- When was the last time you wrote an IPP for yourself...give it a try, it'll make you feel like a kid again. 
- If bored create unattainable goals for the trouble kids, like 'Timmy should persue being a doctor, he really is good at it' even though he gets like 50%...then sit back and wait for the fun
- If your are having a difficult time coming up with IPPs, come to the back, there'll be a white van...bring $20 (unmarked bills but preferably twoonies) and knock in the theme of the tv show 'Magnum PI'"

### Individual Program Plan - Goals
![Individual Program Plan - Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-goals.png)

### Individual Program Plan - Edit Goals
![Individual Program Plan - Edit Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/Individual-program-plan-edit-goals.png)

### Subscriptions to Reports
![Individual Program Plan - Edit Goals](https://github.com/jaysait/CA-Student-Info-System/blob/master/subscriptions.png)
- allowed teachers/directors to set criteria to receive automatic weekly reports (ie grades below a certain %)

### Create Class
![Create Class](https://github.com/jaysait/CA-Student-Info-System/blob/master/create-class.png)

### Attendance
![Attendance](https://github.com/jaysait/CA-Student-Info-System/blob/master/attendance.png)

### Gradebook
![Gradebook](https://github.com/jaysait/CA-Student-Info-System/blob/master/gradebook.png)
- autosave
- right click meta data

### Export Data
![Export Data](https://github.com/jaysait/CA-Student-Info-System/blob/master/export-data.png)
- could autosuggest and export (spreadsheet) much of the data in the system to be used externally

### Mail Merge
![mail-merge-1](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-1.png)
- users could create a .docx document and mark it with special delimited fields ${} and upload it into the system and it would find them and allow them to match them with data in the system ie. student name, gender, etc
-  basically a mail merge functionality built in using system data and without having to know how to mail merge in MS

![mail-merge-2](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-2.png)

![mail-merge-3](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-3.png)
- then they could apply that for any grade

![mail-merge-4](https://github.com/jaysait/CA-Student-Info-System/blob/master/mail-merge-4.png)
- and the system would generate individual documents for each of the kids with the appropriate data filled in

### Labels
![Labels](https://github.com/jaysait/CA-Student-Info-System/blob/master/address-labels.png)
- allowed flexible labels for various tasks ie address

### Various Stats
![Various Stats](https://github.com/jaysait/CA-Student-Info-System/blob/master/login-stats.png)
- had many different metrics/stats to try and help analyse issues

### Permissions
![Permissions](https://github.com/jaysait/CA-Student-Info-System/blob/master/permissions.png)
- finegrain permissions allowing permitted users to control what groups or individuals saw in the system




## Code Samples
## About Me





