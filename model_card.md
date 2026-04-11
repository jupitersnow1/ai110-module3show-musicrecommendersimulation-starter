# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

Give your model a short, descriptive name.  
Example: **VibeFinder 1.0**  

---

## 2. Intended Use  

Describe what your recommender is designed to do and who it is for. 

Prompts:  

- What kind of recommendations does it generate  
- What assumptions does it make about the user  
- Is this for real users or classroom exploration  

---

## 3. How the Model Works  

Explain your scoring approach in simple language.  

Prompts:  

- What features of each song are used (genre, energy, mood, etc.)  
- What user preferences are considered  
- How does the model turn those into a score  
- What changes did you make from the starter logic  

Avoid code here. Pretend you are explaining the idea to a friend who does not program.

---

## 4. Data  

Describe the dataset the model uses.  

Prompts:  

- How many songs are in the catalog  
- What genres or moods are represented  
- Did you add or remove data  
- Are there parts of musical taste missing in the dataset  

---

## 5. Strengths  

Where does your system seem to work well  

Prompts:  

- User types for which it gives reasonable results  
- Any patterns you think your scoring captures correctly  
- Cases where the recommendations matched your intuition  

---

## 6. Limitations and Bias 

The biggest limitation I found is that the system scores each feature independently — it has no way to tell whether a combination of preferences actually makes sense together. For example, when I tested a "sad folk, high energy" profile, the top pick was correct, but the next results were intense rock and pop songs that matched on energy alone. A real listener in a sad mood would never want those, but the system had no way to filter them out.

Genre is also a hard ceiling. If a user's preferred genre doesn't exist in the catalog — like reggae — the maximum possible score drops to 0.60 regardless of how well everything else matches. This means some users are structurally disadvantaged just because of what's in the dataset, not because of anything they did wrong.

Mood matching is binary — a song either matches or it doesn't. There's no sense of "closeness" between moods, so "chill" and "relaxed" are treated as completely different even though most people would consider them similar. This creates some unintuitive gaps in the rankings.

Finally, the catalog is small and unevenly distributed. Some genres have two or three songs, others only one. Users whose taste aligns with well-represented genres (like lofi or pop) get much better results than users whose taste falls outside what's covered. The system doesn't account for this imbalance at all.

---

## 7. Evaluation  

I tested six user profiles — three core and three edge cases — by running the recommender and checking whether the top results matched what a real listener with those preferences would actually want.

**Core profiles (Jackie, Alex, Sam)** all performed well. Jackie's lofi/chill profile surfaced *Midnight Coding* and *Library Rain* as the top two, which felt right. Alex's pop/happy profile gave *Sunrise City* a perfect 1.00 score, which made sense since it matched on all three features. Sam's rock/intense profile correctly ranked *Storm Runner* first.

**Edge cases revealed the real weaknesses.** Jordan's conflicting profile (sad folk, high energy 0.9) got the right top pick, but songs #2 and #3 were intense rock tracks that had nothing to do with a sad mood — they just happened to match on energy. That was the most surprising result because it showed the system doesn't understand that some combinations of preferences conflict with each other.

Riley's reggae profile was another eye-opener. With no reggae in the catalog, every song started at zero for genre, and the highest score anyone could reach was around 0.56. The system still returned results, but they didn't feel meaningful — it was basically guessing based on mood and energy alone.

I also ran a weight shift experiment — doubling energy to 0.50 and halving genre to 0.20 — to see if the balance of features changed the results. It improved some cases (Riley got slightly better picks) but made Jordan worse. That experiment confirmed that the original weights (genre 0.40, mood 0.35, energy 0.25) were the better choice.

---

## 8. Future Work  

Ideas for how you would improve the model next.  

Prompts:  

- Additional features or preferences  
- Better ways to explain recommendations  
- Improving diversity among the top results  
- Handling more complex user tastes  

---

## 9. Personal Reflection  

A few sentences about your experience.  

Prompts:  

- What you learned about recommender systems  
- Something unexpected or interesting you discovered  
- How this changed the way you think about music recommendation apps  
