# Gator

Command-line RSS Feed Aggregator built in Go that allows users to subscribe to RSS feeds, fetch posts, and manage subscriptions.

This is the starter code used in Boot.dev's [Build a Blog Aggregator in Go](https://www.boot.dev/courses/build-blog-aggregator-golang) course.

---

## Requirements

- Go 1.20+
- PostgreSQL

---

## Installation

1. Clone the repository.

2. Install dependencies:
```bash
go mod download
```

3. Install PostgreSQL:  
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```
4. Configure PostgreSQL:

```bash
sudo service postgresql start
sudo -u postgres psql
```

Inside the psql shell:
```sql
CREATE DATABASE gator;
ALTER USER postgres PASSWORD 'postgres';
\q
```

5. Create configuration file ~/.gatorconfig.json
```json
{
  "db_url": "postgres://postgres:postgres@localhost:5432/gator?sslmode=disable",
  "current_user_name": "your_username"
}
```

6. Run migrations:
```bash
goose -dir ./sql/schema postgres "postgres://postgres:postgres@localhost:5432/gator?sslmode=disable" up

```

---

## Usage

Run Gator:
```bash
go run . <command> [arguments]
```

### Example Session

#### Reset users:
```bash
go run . reset
```

Output:
```bash
Users reset successfully!
```

#### Register:
```bash
go run . register kahya
```

Output:
```bash
User kahya created successfully!
```

#### Login:
```bash
go run . login kahya
```

Output:
```bash
User switched successfully!
```

#### Add feed:
```bash
go run . addfeed "Hacker News RSS" "https://hnrss.org/newest"
```

Output:
```bash
{ID:9b505121-4238-42e9-9df2-8a66616667fd Name:Hacker News RSS Url:https://hnrss.org/newest}
successfully added feed to following
```

#### Follow feed:
```bash
go run . follow "https://hnrss.org/newest"
```

Output:
```bash
feed name: Hacker News RSS, user name: kahya
```

#### View followed feeds:
```bash
go run . following
```

Output:
```bash
feed name: Hacker News RSS
feed name: Lanes Blog
```

#### Aggregate posts:
```bash
go run . agg 5s
```

Output:
```bash
Collecting feeds every 5s
post created
post created
...
```

#### Browse recent posts:
```bash
go run . browse 3
```

Output:
```bash
Found 3 posts for user kahya:
Sun Nov 9 from TechCrunch
--- Apple reportedly plans ambitious satellite-powered iPhone features ---
Link: https://techcrunch.com/2025/11/09/apple-reportedly-plans-ambitious-satellite-powered-iphone-features/
=====================================
Sun Nov 9 from TechCrunch
--- TechCrunch Mobility: Elon Musk’s threats worked ---
Link: https://techcrunch.com/2025/11/09/techcrunch-mobility-elon-musks-threats-worked/
=====================================
Sat Nov 8 from TechCrunch
--- Is Wall Street losing faith in AI? ---
Link: https://techcrunch.com/2025/11/08/is-wall-street-losing-faith-in-ai/
```

## Commands

| Command | Description |
|---------|-------------|
| `register <username>` | Register a new user |
| `login <username>` | Log in as an existing user |
| `reset` | Delete all users from the database |
| `users` | List all registered users |
| `addfeed <feed-name> <feed-url>` | Add a new RSS feed |
| `feeds` | List all available feeds |
| `follow <feed-url>` | Follow a feed |
| `unfollow <feed-url>` | Unfollow a feed |
| `following` | List all feeds you are currently following |
| `agg <interval>` | Aggregate (fetch and store latest posts at a specified interval, e.g., `5s`) |
| `browse [limit]` | Browse recent posts from feeds you follow (optionally limit results) |

