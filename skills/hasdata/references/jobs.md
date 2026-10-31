# Jobs APIs: Indeed & Glassdoor
| `domain` | Country site; `www.indeed.com`, `uk.indeed.com`, `de.indeed.com`. |
Response: `jobs` array with `title`, `company`, `location`, `salary`, `description`, `postedAt`, `link`, `jobKey`. Salary is free-form string; parse with regex.