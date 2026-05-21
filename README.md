# Classroom Export Utility

With the transition of GitHub Classroom, teachers can use this script to download their GitHub Classroom data locally using the CLI.

This Bash script exports GitHub Classroom data locally using the [GitHub Classroom CLI extension](https://github.com/github/gh-classroom).

Teachers can use this script to download all of their Classroom data into a local directory for backup or offline analysis in the following formats:
- Classrooms (JSON)
- Assignments per classroom (JSON)
- Accepted assignments / student submissions per assignment (JSON)
- Grades per assignment (CSV, via gh classroom assignment-grades)

## Prerequisites

| Tool | Command | References |
|------|---------|---------|
| GitHub CLI | `brew install gh` | [Installation Instructions](https://github.com/cli/cli#installation) |
| Classroom CLI extension | `gh extension install github/gh-classroom` | [Documentation](https://docs.github.com/en/education/manage-coursework-with-github-classroom/teach-with-github-classroom/using-github-classroom-with-github-cli#using-the-github-classroom-extension-with-github-cli-) |
| jq command-line JSON processor | `brew install jq` | [About jq](https://jqlang.org/)

You must also be authenticated:

```bash
gh auth login
```

## Usage

```bash
# Make the script executable (first time only)
chmod +x export-classrooms.sh

# Export all classrooms
./export-classrooms.sh

# Export a single classroom by ID
./export-classrooms.sh -c 12345

# Specify a custom output directory
./export-classrooms.sh -o ./my-export
```

## What gets exported

The script creates a timestamped directory (e.g. `classroom-export-20250520-143000/`) with the following structure:

```
classroom-export-20250520-143000/
├── classrooms.json                    # All classrooms
├── classroom-<ID>/
│   ├── classroom.json                 # Classroom details
│   ├── assignments.json               # All assignments in this classroom
│   └── assignment-<ID>/
│       ├── assignment.json            # Assignment details
│       ├── accepted-assignments.json  # Student/group submissions
│       └── grades.csv                 # Grade report (CSV)
```

### Data included

| File | Contents |
|------|----------|
| `classrooms.json` | ID, name, archived status, URL, organization info |
| `classroom.json` | Detailed info for one classroom |
| `assignments.json` | Title, type, deadline, invite link, submission counts, editor, starter code repo |
| `assignment.json` | Detailed info for one assignment |
| `accepted-assignments.json` | Submission status, pass/fail, commit count, grade, student info, repository URLs |
| `grades.csv` | Assignment name, GitHub username, roster identifier, repository, submission timestamp, points awarded/available |

## Options

| Flag | Description | Default |
|------|-------------|---------|
| `-c`, `--classroom-id` | Export only the specified classroom | _(all classrooms)_ |
| `-o`, `--output` | Output directory path | `classroom-export-<timestamp>` |
| `-h`, `--help` | Show help message | |

## Finding your Classroom ID

Run the following command to list your classrooms and their IDs:

```bash
gh classroom list
```

## Additional resources

- [Classroom CLI documentation](https://docs.github.com/en/education/manage-coursework-with-github-classroom/teach-with-github-classroom/using-github-classroom-with-github-cli)
- [Classroom REST API](https://docs.github.com/en/rest/classroom/classroom?apiVersion=2022-11-28)
- [GitHub Classroom](https://classroom.github.com)
