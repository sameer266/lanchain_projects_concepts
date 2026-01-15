

---

# Job Notification System (AI-Powered)

A system that **notifies registered users about relevant job opportunities on WhatsApp** using AI-powered job matching. This project leverages **LangChain** and **Ollama/DeepSeek** for semantic matching between user skills and job descriptions.

---

## **Features**

* User registration with skills, preferences, and WhatsApp number.
* Automatic job collection from multiple sources (APIs, websites).
* AI-powered matching of jobs to user profiles.
* Instant WhatsApp notifications for matched jobs.
* Easy to extend for more sources or notification channels.

---

## **Tech Stack**

| Component       | Technology / Tool                          |
| --------------- | ------------------------------------------ |
| Backend         | Python (FastAPI / Django REST)             |
| AI Matching     | LangChain + Ollama / DeepSeek              |
| Database        | PostgreSQL / MongoDB (with vector support) |
| Job Sources     | APIs / Web Scraping                        |
| Notifications   | Twilio WhatsApp API                        |
| Background Jobs | Celery + Redis                             |

---

## **How It Works**

1. **User Registration**
   Users create a profile with skills, location, job type, and WhatsApp number.

2. **Job Collection**
   Jobs are fetched from APIs or scraped from websites and stored in the database.

3. **AI Matching**

   * User profiles and job descriptions are converted into **vector embeddings**.
   * Semantic similarity is computed to find the most relevant jobs.

4. **Notifications**
   Matched jobs are sent as **WhatsApp messages** to users automatically.

---

## **Example Workflow**

1. **User Profile**

```json
{
  "name": "Sameer Baiju",
  "phone_number": "+9779812345678",
  "skills": ["Python", "Django", "API Development"],
  "location": "Remote",
  "preferences": {"job_type": "Full-time", "salary_range": "50k-80k"}
}
```

2. **Job Listing**

```json
{
  "title": "Backend Developer",
  "company": "TechCorp",
  "description": "Looking for Python/Django developer to build scalable APIs.",
  "location": "Remote",
  "salary": "60k"
}
```

3. **Notification Sent**

```
Hi Sameer! New job matching your profile:
- Title: Backend Developer
- Company: TechCorp
- Location: Remote
- Apply here: <job-link>
```

---

## **Setup Instructions**

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/job-notifier.git
cd job-notifier
```

2. **Install dependencies**

```bash
pip install -r requirements.txt
```

3. **Configure environment variables**

```env
DATABASE_URL=postgresql://user:password@localhost:5432/jobdb
TWILIO_SID=your_twilio_sid
TWILIO_AUTH_TOKEN=your_twilio_auth_token
TWILIO_WHATSAPP_NUMBER=whatsapp:+1234567890
```

4. **Run the application**

```bash
uvicorn main:app --reload
```

5. **Schedule job matching and notifications**

* Use Celery with Redis or a cron job to run the matching algorithm and send notifications.

---

## **Future Improvements**

* Add **job ranking** based on user preferences.
* Allow users to **provide feedback** on notifications for better AI matching.
* Extend to **email notifications** or **mobile push notifications**.
* Build an **admin panel** to monitor users and jobs.

---


