# GitHub Classroom Checklist

## To create an assignment:

1. Visit this semester's classroom in [GitHub Classroom](https://classroom.github.com/classrooms).

2. Select New Assignment, obviously.

3. Page 1
   - Set a title with a prefix of “Activity:”, “Exercise:” (for take-home exercises), or “HW0” (replacing 0 with the homework number)
   - For in-class activities:
     - Group assignment
     - “Name your set of teams” → Use activity name
     - For **Critters** only: set “Maximum teams” to 1 (Careful! That’s “Maximum teams,” not “Maximum members per team”) (Future note: we might want to change Critters to have one team per section instead of one for all students. Note that this gets messy in GH Classroom — not clear as of this writing how one instructor can create multiple teams — and would require an update to the activity instructions.)
   - For take-home exercises and homeworks:
     - Individual assignment

4. Page 2
   - Repository: search in the field, or copy & paste org + repo name
     (e.g. `mac-comp127/expressions-and-asts`) from
     https://github.com/mac-comp127 or from `repo:` field in the front matter at the top of the assignment `.md` file
   - Visibility: private
   - Editor: don't use an online IDE
   - (All other settings = default: no admin, default branch only)

5. Page 3
   - Enable feedback pull requests (though we rarely use them for ICAs, enable for all assignment types anyway)
   - (All other settings = default: no autograde, no protected paths)

6. Copy assignment invite link from Classroom into [src/data/assignments/github_classroom_urls.yaml](src/data/assignments/github_classroom_urls.yaml).

7. Commit that change and push to deploy the change.

8. For **Critters**:
   - Use the assignment invite link to accept the assignment yourself (Yes, as the instructor!)
   - Create a team named “All students”
   - Accept the assignment. (It tells you the repository will use your username, but it’s lying. Go ahead and create it anyway.)
