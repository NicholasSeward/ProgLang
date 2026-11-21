# Go — Mini Assignment

**Course:** Programming Languages  
**Accept assignment:** [classroom50.org — Go](https://classroom50.org/Seward-Classes/programming-languages/assignments/hello_go/accept)  
**Submit:** the Git repository link from that accept flow

Go is known for cheap concurrency. You get a **sequential web crawler** that already times itself. Your job is to make the fetches **concurrent** with goroutines and measure the speedup.

---

## Where to work

Accept the assignment and open the repo in a Codespace (or use local Go 1.21+).

```sh
go run main.go
```

| Read when stuck | |
| --- | --- |
| Tour of Go — concurrency | [go.dev/tour/concurrency](https://go.dev/tour/concurrency) |
| `net/http` | [pkg.go.dev/net/http](https://pkg.go.dev/net/http) |
| `sync.Mutex` | for a shared visited set once you go parallel |

---

## Target site

Crawl **[quotes.toscrape.com](https://quotes.toscrape.com/)** — a small site built for scraping practice. Stay on that host. Do not hammer random sites.

| Setting | Value |
| --- | --- |
| Start URL | `https://quotes.toscrape.com/` |
| Max depth | **3** (start page is depth 0) |
| Same host only | yes |

Depth 3 is enough to show a timer difference once pages fetch in parallel.

---

## Starter: sequential crawler

Put this in **`main.go`**. Run it once and keep the printed elapsed time for your README.

```go
package main

import (
	"fmt"
	"io"
	"net/http"
	"net/url"
	"regexp"
	"time"
)

const (
	startURL = "https://quotes.toscrape.com/"
	maxDepth = 3
)

var hrefRe = regexp.MustCompile(`(?i)href\s*=\s*"([^"]+)"`)

func main() {
	start := time.Now()
	visited := make(map[string]struct{})
	bestDepth := make(map[string]int)
	crawl(startURL, 0, visited, bestDepth)
	fmt.Printf("pages: %d\n", len(visited))
	fmt.Printf("elapsed: %v\n", time.Since(start))
}

func crawl(raw string, depth int, visited map[string]struct{}, bestDepth map[string]int) {
	if depth > maxDepth {
		return
	}
	u, err := url.Parse(raw)
	if err != nil {
		return
	}
	u.Fragment = ""
	key := u.String()
	if previousDepth, ok := bestDepth[key]; ok && previousDepth <= depth {
		return
	}
	visited[key] = struct{}{}
	bestDepth[key] = depth

	fmt.Printf("depth %d  %s\n", depth, key)

	body, err := fetch(key)
	if err != nil {
		fmt.Printf("  error: %v\n", err)
		return
	}

	if depth == maxDepth {
		return
	}
	for _, link := range linksOn(key, body) {
		crawl(link, depth+1, visited, bestDepth)
	}
}

func fetch(raw string) (string, error) {
	resp, err := http.Get(raw)
	if err != nil {
		return "", err
	}
	defer resp.Body.Close()
	b, err := io.ReadAll(resp.Body)
	if err != nil {
		return "", err
	}
	return string(b), nil
}

func linksOn(page string, body string) []string {
	base, err := url.Parse(page)
	if err != nil {
		return nil
	}
	seen := map[string]bool{}
	var out []string
	for _, m := range hrefRe.FindAllStringSubmatch(body, -1) {
		ref, err := url.Parse(m[1])
		if err != nil {
			continue
		}
		abs := base.ResolveReference(ref)
		if abs.Host != base.Host {
			continue
		}
		if abs.Scheme != "http" && abs.Scheme != "https" {
			continue
		}
		abs.Fragment = ""
		s := abs.String()
		if seen[s] {
			continue
		}
		seen[s] = true
		out = append(out, s)
	}
	return out
}
```

You should see many `depth …` lines, then `pages: …` and `elapsed: …`.

---

## Your job — make it concurrent

Keep **`main.go`** as the program students run. Change the crawler so that **fetches for different URLs can overlap** using **goroutines** (and channels and/or `sync.WaitGroup` / `sync.Mutex` as you need).

Still required:

| Rule | |
| --- | --- |
| Same start URL and `maxDepth = 3` | |
| Same host only | |
| Do not revisit a URL | shared `visited` needs a **mutex** once several goroutines touch it |
| Print elapsed time | `time.Since` at the end, same idea as the starter |
| Print page count | how many unique URLs you fetched (or marked visited) |

You may reorganize `crawl` / workers however you like. The sequential recursive shape above is only the starting point.

**Do not** keep the sequential one-at-a-time fetch loop and only sprinkle a useless `go` that does not overlap network waits.

---

## What to put in the repo

| Item | |
| --- | --- |
| `main.go` | your **concurrent** crawler (this is what `go run main.go` runs) |
| `README.md` | layout below |

Optional: keep a copy of the sequential starter as `sequential.go` or paste its `elapsed` into the README — not required if the README has both timings.

### README.md

```text
# Go Mini Assignment

## How to run
go run main.go

## Timing
Sequential elapsed: …
Concurrent elapsed: …
Page count (sequential): …
Page count (concurrent): …

## Reflection
1. What surprised you about Go?
2. Which Sebesta criterion (readability, writability, reliability, cost) does it favor, and what does it give up?
3. Where did the speedup come from? What still has to be synchronized?

## AI / assistance
whether you used AI tools, what for, and that you can explain every line you submit
```

Run the starter **before** you change it (or keep the sequential timing from a first run). Then run your concurrent `main.go`. Both elapsed times belong in the README.

---

## Grading

| Result | Score |
| --- | --- |
| Meets the spec | **100** |
| Each minor issue | **−10** |
| More than **3** issues | **Kicked back** |

### To spec

- `main.go` is a depth-3 crawler of `quotes.toscrape.com` with overlapping fetches via goroutines
- README has sequential vs concurrent timings and the Reflection / AI sections

### Minor issues

- Page counts differ slightly between runs (site / link discovery) but both runs complete
- Noisy printing but timing is clear

### Kick-back

- Still sequential (no overlapping fetches)
- Wrong file name (must be runnable as `go run main.go`)
- No timing comparison in the README
- Reflection or AI disclaimer missing from `README.md`
