# AUDIT – fullstack-audit

Granskning av startprojektet (.NET 10 API + React) gjord som "ny kollega" som precis fått repot.

## Steg 1: Starta från noll

Klonade repot till en tom mapp och följde README.

- README säger vilka verktyg som behövs (.NET 10, Node 22+) och vilka kommandon som startar backend och frontend.
- README säger inte att backend och frontend måste köras i två terminaler samtidigt.
- README säger inte i vilken ordning man ska starta dem, eller hur man ser att det fungerar (t.ex. öppna `/health`).
- Kommandon som ska köras i olika mappar (`backend`, `frontend`) är lätta att blanda ihop. Jag klistrade in två kommandon på samma rad och fick fel.

## Checklista (0 = saknas, 1 = finns lite, 2 = sitter bra)

### Hur koden är byggd

| Punkt | Poäng | Kommentar |
|---|---|---|
| Tunna controllers, logik i services | 2 | `TodosController` har tre metoder på 2–3 rader. Den anropar bara `TodoService` och översätter resultatet till HTTP-statuskoder. |
| DTOs används | 2 | `CreateTodoRequest` används som input och `TodoResponse` som output. Databasentiteten `Todo` skickas aldrig direkt till frontend. |
| Frontend lätt att hitta i | 2 | Uppdelad per feature (`src/features/todos/components`), samma idé som backendens `Features/Todos`. |

### Hur koden mår

| Punkt | Poäng | Kommentar |
|---|---|---|
| Tester som körs med ett kommando | 1 | `dotnet test Starter.slnx` (7 tester) och `npm test` (2 tester) går igenom. Men bara 2 frontendtester är för lite för att lita på. |
| Servern validerar data | 2 | Egen `TodoRules`-klass. API:t svarar `400` om titeln är tom eller för lång. |
| Vettiga felmeddelanden | 1 | Backend returnerar `ProblemDetails` med 400/404. Det är oklart hur bra frontend visar felet för användaren. `List` kollar inte `result.Success` som de andra metoderna gör. |
| Laddning och tom lista i React | 2 | Det finns ett test för "friendly message when there are no todos". |

### Hur repot fungerar för ett team

| Punkt | Poäng | Kommentar |
|---|---|---|
| README räcker | 1 | Bra grund, men saknar två terminaler, startordning och hur man vet att det fungerar. |
| bin/obj/node_modules utanför git | 2 | Finns i `.gitignore`, visas gråa i VS Code. Även `starter.db` ignoreras. |
| Inga lösenord/nycklar | 2 | Sökte efter `password` och `secret` i koden och i hela git-historiken (`git log -p`). Inga träffar. |
| Bra commit-historik | 1 | Bara en commit, "Initial commit", eftersom repot skapades från en template. Historiken visar inte hur projektet växte fram. |
| Konsekvent formatering | 2 | `dotnet format --verify-no-changes`, `npm run format:check` och `npm run lint` ger inga fel. |

**Totalt: 23 / 30**

## Mina 5 viktigaste förbättringar

1. **Automatiska kontroller i GitHub Actions.** Inget kör bygg, tester och formatering automatiskt idag. En PR ska stoppas om något går sönder.
2. **Fler frontendtester.** Bara 2 tester. Saknar t.ex. test för att skapa en todo, markera som klar och felhantering när API:t inte svarar.
3. **Tydligare README.** Lägg till att backend och frontend körs i två terminaler, i vilken ordning, och hur man kontrollerar att allt fungerar (`/health` och `localhost:5173`).
4. **Konsekvent felhantering.** `List` i `TodosController` ska kolla `result.Success` som de andra metoderna, och frontend ska visa ett tydligt meddelande när backend inte svarar.
5. **Bättre commit-historik framöver.** Små commits med tydliga meddelanden, en per issue, via Pull Requests.

## AI-användning

Jag använde Claude som stöd under granskningen.

- **Höll med om:** att controllern är tunn och att DTOs används rätt, att 2 frontendtester är för lite, att README saknar info om två terminaler.
- **Valde bort:** att räkna "saknar delete-endpoint" som ett kvalitetsproblem. Det är en saknad funktion, inte dålig kvalitet.