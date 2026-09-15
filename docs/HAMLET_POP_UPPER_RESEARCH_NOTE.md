# Hamlet Pop-Upper — artistic research note

## The popup as performer

*Hamlet Pop-Upper* works with the popup as a procedure: it interrupts another activity, enters the foreground, requests a selection, withdraws, and returns under conditions set by software. The current artifact performs this procedure through an HTML `<dialog>`. When opened with `showModal()`, the dialog enters the browser's top layer and the rest of the document becomes inert for the duration of the modal state ([MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDialogElement/showModal)). The popup is therefore treated as an apparatus that redistributes attention and action, rather than as a visual quotation of an older browser window.

Several documented moments give this apparatus a history without reducing that history to one linear succession. Ethan Zuckerman dates his Tripod popup work to late 1996 or early 1997 and describes a window that kept advertising attached to a user's page while placing it outside the page's body ([Zuckerman](https://ethanzuckerman.com/about-me/)). Mozilla's release record lists popup blocking among the features of Phoenix 0.4 in October 2002 ([Mozilla](https://www-archive.mozilla.org/news.html)). Lokesh Dhakar describes the first 2005 version of Lightbox as a "faux-popup effect" made inside the page, together with early questions about how visitors would close it ([Dhakar](https://github.com/lokesh/lightbox/blob/main/README.md)). As documented by MDN on 15 September 2026, each call to `window.open()` generally requires its own transient user activation ([MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Window/open)). These cases document different technical bodies and rules for the same family of actions: separation, foregrounding, interruption, closure, and suppression.

The artwork transfers this family of actions into dramaturgy. The popup receives Hamlet's question and becomes the surface from which the question is repeatedly addressed. "Remember this answer" retains a selected value for a later opening. "Ask me later" schedules another opening while the death score keeps its own time. Memory and deferral operate differently: one carries a past answer forward; the other gives the unresolved request a future interval.

## Remembering a decision

Joel Weinberger and Adrienne Porter Felt's 2016 study, *A Week to Remember*, examines how long a browser should retain a person's decision to override an HTTPS warning. Repeated warnings can reduce trust and attention, while lengthy storage increases the duration of a mistaken decision. Their multi-month field experiment covered 1,614,542 Chrome warning impressions and compared policies ranging from a browser session to three months ([Weinberger and Felt 2016](https://www.usenix.org/system/files/conference/soups2016/soups2016-paper-weinberger.pdf)).

*Hamlet Pop-Upper* transfers the storage operation into another field. In the security study, an expiration policy balances warning adherence and the cost of a mistake. In the artwork, the stored object is an answer to "To be or not to be." The interface gives that answer continuity as a value. Each return then places the same value inside another moment, where the person, the question, and the approaching death score give it another authority.

Deferral composes the other temporal operation. "Ask me later" assigns the request another time while preserving its return. A death may arrive inside the resulting interval, so the software's schedule and the play's score meet without becoming one clock. The research question becomes executable: what happens to an answer when software carries it forward while dramatic time continues to change the conditions of its use?

## Repetition and habituation

Anthony Vance and colleagues' 2019 study, *The Fog of Warnings*, examines generalization of habituation across interface notifications. Their experiment found that repeated exposure to an ordinary notification reduced attention and adherence when participants later encountered a visually similar security warning ([Vance et al. 2019](https://www.usenix.org/conference/soups2019/presentation/vance)). This finding establishes a risk attached to recurrent interface forms: familiarity with the form can shape attention before the content of a later warning is considered.

The artwork stages that risk rather than measuring it. Its repeated question remains available to conviction, impatience, habit, deferral, and change. The cited study establishes an empirical result within usable-security research; the artwork gives recurrence a dramatic duration in which an answer and the act of answering can separate.

## Where the research enters the artifact

The work begins with "Father is dead." Seven further timed events follow: Polonius, Ophelia, Mother, Claudius, Rosencrantz and Guildenstern, Laertes, and Hamlet. Before each event joins the visible list, the program records the condition in which it reached the dialog:

- `unanswered` — the question was open and waiting;
- `answer_collision` — the death arrived while a selection was being registered;
- `deferred` — the death crossed an active "Ask me later" interval;
- `recalled` — the dialog was closed while a remembered answer remained active;
- `answered` — the dialog was closed after an unremembered answer;
- `closed` — the dialog was closed without one of the more specific conditions;
- `terminal` — Hamlet's final event.

During each event, its death sentence appears briefly inside the dialog. After the sentence joins the list on the page, a thin line remains on the dialog surface; its position, length, and density vary with the recorded condition. A second line connects the listed death back to the place where it met the dialog. The classification is therefore visible as a different material relation between death and apparatus, rather than as metadata presented to the visitor.

This mechanism turns the research relation into executable material. A stored answer can enter several later encounters because its meaning is produced together with machine state and event time. A deferral can be crossed by a death whose schedule was already running. The fixed score remains legible as Shakespearean constraint, while the traces record how interface time met it.

The work therefore investigates recurrence through controlled difference. The words of the question, the death order, and the stored values are stable elements; timing and encounter state redistribute what those elements do together. The artifact makes one relation observable: the same stored value acquires different dramatic force according to when it returns, which condition carries it, and which death meets it. The death score supplies constraint; the visitor's selections and deferrals supply variation. Research occurs in the repeated handling of those materials, where the same components return and accumulate different consequences.

## References

1. William Shakespeare, *Hamlet*, Folger Shakespeare Library: https://www.folger.edu/explore/shakespeares-works/hamlet/read/
2. Ethan Zuckerman, "About Me," "Much older work: Tripod": https://ethanzuckerman.com/about-me/
3. Mozilla.org, archived news, Phoenix 0.4 release, 29 October 2002: https://www-archive.mozilla.org/news.html
4. Lokesh Dhakar, *Lightbox* repository history: https://github.com/lokesh/lightbox/blob/main/README.md
5. MDN Web Docs, `Window.open()`, accessed 15 September 2026: https://developer.mozilla.org/en-US/docs/Web/API/Window/open
6. MDN Web Docs, `HTMLDialogElement.showModal()`, accessed 15 September 2026: https://developer.mozilla.org/en-US/docs/Web/API/HTMLDialogElement/showModal
7. Joel Weinberger and Adrienne Porter Felt, "A Week to Remember: The Impact of Browser Warning Storage Policies," SOUPS 2016: https://www.usenix.org/system/files/conference/soups2016/soups2016-paper-weinberger.pdf
8. Anthony Vance et al., "The Fog of Warnings: How Non-essential Notifications Blur with Security Warnings," SOUPS 2019: https://www.usenix.org/conference/soups2019/presentation/vance
9. Mohammad Zare, *Hamlet Pop-Upper*, source and change plan: https://github.com/mozareeduge/HAMLET-POP-UPPER
