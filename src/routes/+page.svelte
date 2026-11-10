<script lang="ts">
  import 'bootstrap/dist/css/bootstrap.min.css';

  type Question = {
    id: string;
    statement: string;
    references?: string[];
  };

  type Section = {
    title: string;
    passage: string;
    introduction?: string;
    questions: Question[];
  };

  const scores = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

  const sections: Section[] = [
    {
      title: 'Faith in Jesus Christ',
      passage: '1 John 4:2–6',
      questions: [
        {
          id: 'jesus-born',
          statement: 'I believe that Jesus was born into this world as the Son of God.',
          references: ['Isaiah 9:6', 'John 1:1', 'John 1:14']
        },
        {
          id: 'jesus-lord',
          statement: 'Above my own life, I proclaim that Jesus Christ is my Lord and Savior.',
          references: ['Isaiah 53:5']
        }
      ]
    },
    {
      title: 'Love Toward Others',
      passage: '1 John 4:7–19',
      introduction: 'The love I received from God is also reflected towards others in these ways:',
      questions: [
        { id: 'patient', statement: 'I am patient: I am slow to anger and willing to endure the faults and difficulties of others.' },
        { id: 'kind', statement: 'I am kind: I actively show compassion, generosity, and goodness toward others.' },
        { id: 'no-envy', statement: 'I do not envy: I do not resent the blessings, abilities, or successes of others.' },
        { id: 'no-boasting', statement: 'I do not boast: I do not seek praise or attention by elevating myself above others.' },
        { id: 'not-proud', statement: 'I am not proud: I remain humble and do not consider myself superior to others.' },
        { id: 'not-rude', statement: 'I am not rude: I treat others with dignity and respect, even when I disagree with them.' },
        { id: 'not-self-seeking', statement: 'I am not self-seeking: I seek the good of others rather than insisting on having my own way.' },
        { id: 'not-easily-angered', statement: 'I am not easily angered: I am not easily provoked or offended, and I exercise self-control when upset.' },
        { id: 'forgive', statement: 'I do not keep a record of wrongs: I forgive others and do not continually hold their past offenses against them.' },
        { id: 'no-evil', statement: 'I do not rejoice in evil: I do not take pleasure in sin, injustice, or the suffering of others.' },
        { id: 'truth', statement: 'I rejoice in the truth: I delight in what is true, righteous, and pleasing to God.' },
        { id: 'bear', statement: 'I bear all things: I am willing to endure hardships and difficulties for the good of others.' },
        { id: 'believe', statement: 'I believe all things: I give others the benefit of the doubt without ignoring truth or wisdom.' },
        { id: 'hope', statement: "I hope for all things: I maintain hope in God's goodness and His ability to work in difficult situations." },
        { id: 'endure', statement: 'I endure all things: I remain faithful in love even through suffering, disappointment, and adversity.' },
        { id: 'no-fear', statement: 'I have no fear in my love for God: I am confident in ministering to others as my faith is well founded in God’s Word.' }
      ]
    },
    {
      title: 'Love and Obedience',
      passage: '1 John 4:20–21; 1 John 5:1–5',
      introduction: 'I love God and keep His commandments:',
      questions: [
        { id: 'no-other-gods', statement: 'I have no other gods before God: I put God first in my life and do not allow anything or anyone to take His place in my worship, devotion, or priorities.' },
        { id: 'no-idols', statement: 'I do not worship idols: I worship God alone and do not make or use images, objects, or anything created as an object of worship.' },
        { id: 'name', statement: 'I do not take God’s name in vain: I treat God’s name with reverence and respect, avoiding careless, dishonest, or disrespectful use of His name. I do not use God’s name against His will.' },
        { id: 'sabbath', statement: 'I remember the Sabbath day and keep it holy: I honor the seventh-day Sabbath (Saturday) by resting from ordinary work, worshiping God, and setting the day apart for Him.' },
        { id: 'parents', statement: 'I honor my father and mother: I treat my parents with respect, gratitude, and care, recognizing their role in my life.' },
        { id: 'murder', statement: 'I do not murder: I respect the sacredness of human life and avoid hatred, cruelty, and unjust harm toward others.' },
        { id: 'adultery', statement: 'I do not commit adultery: I remain faithful in marriage and strive for purity in my thoughts, desires, and actions. This also pertains to sexual purity and immorality.' },
        { id: 'steal', statement: 'I do not steal: I respect the property and rights of others and do not take what does not belong to me or gain anything through dishonesty.' },
        { id: 'false-witness', statement: 'I do not bear false witness: I speak truthfully about others and avoid lying, spreading false accusations, or misrepresenting people.' },
        { id: 'covet', statement: 'I do not covet: I am content with what God has given me and do not desire to possess what belongs to others out of jealousy or selfishness.' },
        { id: 'love-others', statement: 'Considering commandments 5–10, I love others as Jesus has loved me: I treat others with selfless love, compassion, patience, forgiveness, and humility. I seek their well-being, even when doing so requires personal sacrifice, and I demonstrate my love through actions rather than words alone.' }
      ]
    }
  ];

  const rebukes = [
    'Loss of love for Christ',
    'False teachings',
    'Idolatry',
    'Sexual immorality',
    'Tolerance of sin',
    'Spiritual complacency',
    'Spiritual deadness',
    'Incomplete obedience to God',
    'Lukewarm faith',
    'Spiritual pride',
    'Self-sufficiency',
    'Lack of repentance',
    'Spiritual blindness',
    'Compromise with worldly influences'
  ];

  let ratings = $state<Record<string, number | undefined>>({});
  const total = sections.reduce((count, section) => count + section.questions.length, 0);
  let completed = $derived(
    sections.reduce(
      (count, section) =>
        count + section.questions.filter((question) => ratings[question.id] !== undefined).length,
      0
    )
  );

  // Recalculate total score whenever a rating changes.
  const maximumScore = total * 10;
  let totalScore = $derived(
    sections.reduce(
      (sum, section) =>
        sum + section.questions.reduce((points, question) => points + (ratings[question.id] ?? 0), 0),
      0
    )
  );
  let percentage = $derived(Math.round((totalScore / maximumScore) * 100));
</script>

<svelte:head>
  <title>Criteria - The Spirit of God</title>
  <meta name="description" content="A biblical self-reflection questionnaire based on 1 John 4–5." />
</svelte:head>

<main class="container py-4 py-md-5" style="max-width: 900px;">
  <header class="text-center mb-5">
    <h1 class="display-6 fw-bold">Criteria - The Spirit of God</h1>
    <p class="lead mt-3">
      “Beloved, do not believe every spirit, but test the spirits, whether they are of God; because many false prophets have gone out into the world.”
    </p>
    <p class="text-secondary">1 John 4:1</p>
  </header>

  <div class="alert alert-light border mb-4" role="note">
    <h2 class="h5">Instructions</h2>
    <p class="mb-2">Rate each statement from <strong>1 to 10</strong>: 1 means you fully disagree and 10 means you fully agree.</p>
    <span class="small text-secondary">{completed} of {total} statements rated</span>
  </div>

  {#each sections as section (section.title)}
    <section class="card mb-4">
      <div class="card-header bg-light py-3">
        <h2 class="h4 mb-1">{section.title}</h2>
        <span class="text-secondary small">{section.passage}</span>
      </div>
      {#if section.introduction}
        <div class="card-body pb-0">
          <p class="mb-3">{section.introduction}</p>
        </div>
      {/if}
      <div class="list-group list-group-flush">
        {#each section.questions as question (question.id)}
          <div class="list-group-item p-3">
            <div class="d-flex flex-column flex-sm-row align-items-sm-center justify-content-between gap-3">
              <div>
                <label class="mb-0" for={question.id}>{question.statement}</label>
                {#if question.references}
                  <div class="small text-secondary mt-1">{question.references.join(' · ')}</div>
                {/if}
              </div>
              <select
                class="form-select flex-shrink-0"
                style="width: 110px;"
                id={question.id}
                bind:value={ratings[question.id]}
              >
                <option value={undefined} disabled>Rate</option>
                {#each scores as score}
                  <option value={score}>{score}/10</option>
                {/each}
              </select>
            </div>
          </div>
        {/each}
      </div>
    </section>
  {/each}

  <section class="card mb-4">
    <div class="card-body">
      <h2 class="h4">Submit Yourself to God</h2>
      <p>A broken and guilty heart are the sacrifices of God.</p>
      <p>As you go over how well your spirit aligns with God, do not focus on how you can improve. Rather, focus on how you can submit your entire self to God.</p>
      <blockquote class="border-start border-3 ps-3 my-4">
        <p class="mb-1">“The sacrifices of God are a broken spirit: a broken and a contrite heart, O God, thou wilt not despise.”</p>
        <footer class="text-secondary small">Psalm 51:17</footer>
      </blockquote>
      <blockquote class="border-start border-3 ps-3 mb-0">
        <p class="mb-1">“But now, O LORD, thou art our father; we are the clay, and thou our potter; and we all are the work of thy hand.”</p>
        <footer class="text-secondary small">Isaiah 64:8</footer>
      </blockquote>
    </div>
  </section>

  <section class="card mb-5">
    <div class="card-body">
      <h2 class="h4">The Seven Churches</h2>
      <p class="text-secondary small">Revelation 2–3</p>
      <p>Read over the seven churches. Notice how only two churches (Smyrna and Philadelphia) were not rebuked by God. These two churches were known to be faithful to God and thus were persecuted.</p>
      <blockquote class="border-start border-3 ps-3 my-4">
        <p class="mb-1">“If the world hate you, ye know that it hated me before it hated you. If ye were of the world, the world would love his own: but because ye are not of the world, but I have chosen you out of the world, therefore the world hateth you. Remember the word that I said unto you, The servant is not greater than his lord. If they have persecuted me, they will also persecute you.”</p>
        <footer class="text-secondary small">John 15:18–20</footer>
      </blockquote>
      <h3 class="h5">What were the other five churches rebuked for?</h3>
      <ul class="mb-0">
        {#each rebukes as rebuke}
          <li class="mb-1">{rebuke}</li>
        {/each}
      </ul>
    </div>
  </section>

  <section class="card mb-5" aria-live="polite">
    <div class="card-body text-center py-4">
      <h2 class="h4 mb-3">Final Score</h2>
      {#if completed === total}
        <p class="display-5 fw-bold mb-1">{totalScore} / {maximumScore}</p>
        <p class="fs-4 mb-0">{percentage}%</p>
      {:else}
        <p class="fs-5 fw-semibold mb-2">{totalScore} / {maximumScore} points so far</p>
        <p class="text-secondary mb-0">
          {completed} of {total} statements rated. Complete all statements to see your final score.
        </p>
      {/if}
    </div>
  </section>
</main>
