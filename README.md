# Class attendance register

A register for one class. The tutor marks who turned up, each learner
sees their own record and the rate it adds up to, a mistake can be put
right where everybody can see it was, and the whole term comes out as
a file an administrator can act on.

## What finished looks like

A stranger can open the URL, register and come back to their own
account. A tutor sets up a class, its learners and the days it meets,
then marks every learner present, late, absent or excused, and puts a
mistake right afterwards where the learner can see it was put right. A
learner opens their own record, sees nobody else's, and reads a rate
that says what it counted. At the end of term the tutor downloads the
whole term as a file that names the dates it covers, agrees with the
screen it was made from, and does not quietly change after it has been
handed in.

Finished does not mean running on a laptop. It means a tutor who has
never met the person who built it can open it on a Monday morning and
take the register.

## The road map

Eight sprints. Each one makes a different part of that sentence true,
and the order is not arbitrary: take any sprint out and the sentence
stops being true.

| # | Sprint | What it makes possible |
|---|---|---|
| 1 | Getting in | Somebody makes an account and comes back to it |
| 2 | A class, and the days it meets | A class with a roster and dated sessions to mark |
| 3 | Four ways to be missing | Present, late, absent and excused are four different things |
| 4 | A number somebody can defend | A rate per learner that states what it counted |
| 5 | Their own record, and nobody else's | A learner reads their own attendance and only their own |
| 6 | Marked absent, was there | A mark can be corrected, and the correction is visible |
| 7 | The term as a file | A term downloads as a file that agrees with the screen |
| 8 | Where a school reaches it | It is on the internet at an address a school could print |

## The hard part

The file. A grid of ticks is an afternoon's work. A file somebody
hands to an administrator is a claim about a term: it has to say which
dates it covers, agree with what the screen said at the moment it was
made, survive a correction landing the week after without silently
rewriting itself, and still be readable in a year by somebody who was
not there.

Underneath it sits the other hard part, which is the number. Present,
late, absent and excused are four different things, and a percentage
computed without deciding what each one does to the denominator is a
number nobody can defend to the learner it judges.

## Working on it

The sprints, their briefs and the tickets under them are in
Blacksmith. Start with sprint one; each sprint opens as the one before
it closes.

The reading attached to a sprint is worth opening before its first
ticket rather than after. It carries the parts a ticket deliberately
does not: what the alternatives were, and which one you are choosing
between.

## What this is built with

The whole product: an API and the screens that use it.

- **Django** with **Django REST Framework** for the API.
- **SimpleJWT** for authentication, against a custom user model in `apps/users`.
- **drf-spectacular** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── config/
│   ├── settings/        # base, development, production
│   └── urls.py          # where routes are mounted
├── apps/
│   └── users/           # the custom user model, and auth
├── utils/               # shared helpers, base model
├── manage.py
└── requirements.txt

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a serializer or a route. |
| `blacksmith make:resource Post` | Scaffold a model, serializer, viewset and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run a Django management command, e.g. `blacksmith backend createsuperuser`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Django and React project. Nothing here is a dependency on us. |
