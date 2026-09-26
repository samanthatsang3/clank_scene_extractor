<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Installation and usage guide for the Clank Scene Indexer Chrome extension.">
  <title>Clank Scene Indexer — Scene & Popularity Collector</title>
  <style>
    :root {
      --bg: #070b12;
      --bg-soft: #0d1420;
      --panel: #111a29;
      --panel-2: #162236;
      --text: #f4f7ff;
      --muted: #9aabc2;
      --border: rgba(255,255,255,.1);
      --accent: #76a9ff;
      --accent-2: #9d7cff;
      --green: #6ce0a2;
      --yellow: #f3cc72;
      --pink: #e4a1ff;
      --danger: #ff9a9a;
      --shadow: 0 22px 70px rgba(0,0,0,.35);
      --radius: 18px;
      --content: 1040px;
    }

    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }

    body {
      margin: 0;
      color: var(--text);
      background:
        radial-gradient(circle at 15% 0%, rgba(77,117,190,.18), transparent 32%),
        radial-gradient(circle at 85% 10%, rgba(142,92,255,.10), transparent 28%),
        var(--bg);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      line-height: 1.65;
    }

    a { color: var(--accent); text-decoration: none; }
    a:hover { text-decoration: underline; }

    .top-links {
      position: fixed;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      z-index: 1000;
      width: min(var(--content), calc(100% - 32px));
      margin: 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      pointer-events: none;
    }

    .top-links > a {
      pointer-events: auto;
    }

    .creator-link-card {
      order: 2;
      display: grid;
      grid-template-columns: 46px minmax(0, 1fr);
      gap: 11px;
      align-items: center;
      width: min(100%, 255px);
      padding: 10px 12px;
      border: 1px solid rgba(118,169,255,.33);
      border-radius: 14px;
      background: linear-gradient(145deg, rgba(22,36,61,.98), rgba(14,26,47,.98));
      box-shadow: 0 10px 28px rgba(0,0,0,.28);
      color: var(--text);
      text-decoration: none;
    }

    .creator-link-card:hover {
      text-decoration: none;
      border-color: rgba(118,169,255,.58);
      transform: translateY(-1px);
    }

    .creator-link-card img {
      width: 42px;
      height: 42px;
      object-fit: cover;
      border-radius: 50%;
      border: 1px solid rgba(255,255,255,.22);
      box-shadow: 0 0 0 2px rgba(0,0,0,.18);
    }

    .creator-link-meta {
      min-width: 0;
      text-align: right;
    }

    .creator-link-kicker {
      display: block;
      margin-bottom: 1px;
      color: #8ba9dd;
      font-size: 8px;
      font-weight: 900;
      letter-spacing: .18em;
      text-transform: uppercase;
    }

    .creator-link-name {
      display: block;
      color: #f6f8ff;
      font-size: 13px;
      font-weight: 850;
      line-height: 1.15;
    }

    .creator-link-url {
      display: block;
      margin-top: 4px;
      overflow: hidden;
      color: #86a9e8;
      font-size: 7px;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .home-link {
      order: 1;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      min-height: 40px;
      padding: 9px 13px;
      border: 1px solid var(--border);
      border-radius: 11px;
      background: rgba(255,255,255,.035);
      color: #dbe6f8;
      font-size: 11px;
      font-weight: 900;
      letter-spacing: .08em;
      text-decoration: none;
    }

    .home-link:hover {
      text-decoration: none;
      border-color: rgba(118,169,255,.42);
      background: rgba(118,169,255,.08);
    }

    .creator-credit {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 8px 12px;
      margin: 10px 0 0;
      color: #aebbd0;
      font-size: 13px;
      font-weight: 700;
    }

    .creator-credit a {
      font-weight: 850;
    }

    .back-home-inline {
      display: inline-flex;
      align-items: center;
      padding: 4px 8px;
      border: 1px solid rgba(118,169,255,.24);
      border-radius: 8px;
      background: rgba(118,169,255,.06);
      color: #9fc0ff;
      font-size: 11px;
      letter-spacing: .06em;
    }

    .page {
      width: min(var(--content), calc(100% - 32px));
      margin: 0 auto;
      padding: 100px 0 72px;
    }

    .hero {
      position: relative;
      overflow: hidden;
      padding: 42px;
      border: 1px solid var(--border);
      border-radius: 26px;
      background: linear-gradient(145deg, rgba(20,32,52,.98), rgba(10,16,27,.98));
      box-shadow: var(--shadow);
    }

    .hero::after {
      content: "";
      position: absolute;
      inset: auto -120px -140px auto;
      width: 360px;
      height: 360px;
      border-radius: 999px;
      background: radial-gradient(circle, rgba(118,169,255,.22), transparent 65%);
      pointer-events: none;
    }

    .eyebrow {
      margin-bottom: 8px;
      color: var(--accent);
      font-size: 12px;
      font-weight: 900;
      letter-spacing: .16em;
      text-transform: uppercase;
    }

    h1, h2, h3 { line-height: 1.18; }
    h1 { margin: 0; max-width: 820px; font-size: clamp(34px, 6vw, 58px); letter-spacing: -.03em; }
    h2 { margin: 0 0 14px; font-size: clamp(25px, 4vw, 34px); }
    h3 { margin: 24px 0 8px; font-size: 20px; }

    .subtitle {
      max-width: 760px;
      margin: 18px 0 0;
      color: #c7d1e2;
      font-size: 18px;
    }

    .badges {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 22px;
    }

    .badge {
      display: inline-flex;
      align-items: center;
      min-height: 28px;
      padding: 5px 10px;
      border: 1px solid rgba(255,255,255,.09);
      border-radius: 999px;
      background: rgba(255,255,255,.045);
      color: #cbd6e7;
      font-size: 12px;
      font-weight: 800;
    }

    .badge.beta { color: var(--yellow); border-color: rgba(243,204,114,.25); background: rgba(243,204,114,.07); }

    .toc {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      margin: 18px 0 0;
    }

    .toc a {
      display: block;
      padding: 12px 14px;
      border: 1px solid var(--border);
      border-radius: 12px;
      background: rgba(255,255,255,.025);
      color: #d6deec;
      font-weight: 750;
      text-align: center;
    }

    .section {
      margin-top: 22px;
      padding: 30px;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: rgba(13,20,32,.88);
      box-shadow: 0 16px 50px rgba(0,0,0,.18);
    }

    .section > p:first-of-type { margin-top: 0; }

    .callout {
      margin: 18px 0;
      padding: 15px 17px;
      border: 1px solid rgba(118,169,255,.23);
      border-left: 4px solid var(--accent);
      border-radius: 12px;
      background: rgba(73,118,187,.08);
      color: #cbd6e6;
    }

    .callout.warning {
      border-color: rgba(243,204,114,.25);
      border-left-color: var(--yellow);
      background: rgba(243,204,114,.065);
    }

    .callout.private {
      border-color: rgba(228,161,255,.22);
      border-left-color: var(--pink);
      background: rgba(228,161,255,.055);
    }

    ul, ol { padding-left: 22px; }
    li + li { margin-top: 6px; }

    code {
      padding: 2px 6px;
      border: 1px solid rgba(255,255,255,.08);
      border-radius: 6px;
      background: #090f18;
      color: #c8dbff;
      font-family: "SFMono-Regular", Consolas, "Liberation Mono", monospace;
      font-size: .92em;
    }

    pre {
      overflow-x: auto;
      margin: 14px 0;
      padding: 15px 17px;
      border: 1px solid rgba(255,255,255,.08);
      border-radius: 12px;
      background: #090f18;
      color: #cfe0ff;
      font-family: "SFMono-Regular", Consolas, "Liberation Mono", monospace;
      font-size: 13px;
      line-height: 1.55;
      white-space: pre-wrap;
    }

    .steps {
      display: grid;
      gap: 18px;
      margin-top: 18px;
    }

    .step {
      padding: 18px;
      border: 1px solid rgba(255,255,255,.08);
      border-radius: 14px;
      background: var(--panel);
    }

    .step-number {
      display: inline-grid;
      place-items: center;
      width: 30px;
      height: 30px;
      margin-right: 8px;
      border-radius: 999px;
      background: rgba(118,169,255,.14);
      color: var(--accent);
      font-weight: 900;
      vertical-align: middle;
    }

    .download-link {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      margin: 8px 0 14px;
      padding: 11px 16px;
      border: 1px solid rgba(118,169,255,.42);
      border-radius: 11px;
      background: linear-gradient(145deg, rgba(45,91,164,.95), rgba(31,66,121,.95));
      color: #fff;
      font-size: 12px;
      font-weight: 900;
      letter-spacing: .04em;
      text-decoration: none;
      box-shadow: 0 9px 24px rgba(0,0,0,.24);
    }

    .download-link:hover {
      text-decoration: none;
      border-color: rgba(118,169,255,.72);
      filter: brightness(1.08);
    }

    .screenshot {
      margin: 18px 0 2px;
      padding: 26px 20px;
      border: 1px dashed rgba(118,169,255,.42);
      border-radius: 14px;
      background: linear-gradient(145deg, rgba(118,169,255,.06), rgba(255,255,255,.018));
      color: #9bb0ce;
      text-align: center;
    }

    .screenshot strong {
      display: block;
      margin-bottom: 4px;
      color: #dbe7fb;
    }


    .screenshot.image-filled {
      padding: 10px;
      border-style: solid;
      background: rgba(6,10,18,.72);
    }

    .screenshot.image-filled a {
      display: block;
      width: fit-content;
      max-width: 100%;
      margin: 0 auto;
      border-radius: 10px;
      overflow: hidden;
    }

    .screenshot.image-filled img {
      display: block;
      width: auto;
      max-width: 100%;
      height: auto;
      margin: 0 auto;
      border-radius: 10px;
    }

    .feature-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
      margin-top: 14px;
    }

    .feature {
      padding: 14px;
      border: 1px solid rgba(255,255,255,.07);
      border-radius: 12px;
      background: rgba(255,255,255,.025);
    }

    .feature b { display: block; margin-bottom: 4px; }

    .leaderboard {
      margin-top: 14px;
      padding: 16px;
      border: 1px solid rgba(255,255,255,.08);
      border-radius: 12px;
      background: #0b111c;
      font-family: "SFMono-Regular", Consolas, monospace;
      font-size: 13px;
      color: #d7e1f2;
      white-space: pre-wrap;
    }

    .footer {
      margin-top: 26px;
      padding: 18px 4px 0;
      border-top: 1px solid var(--border);
      color: #75869f;
      font-size: 12px;
      text-align: center;
    }

    @media (max-width: 820px) {
      .toc, .feature-grid { grid-template-columns: 1fr 1fr; }
      .hero { padding: 28px; }
      .section { padding: 22px; }
    }

    @media (max-width: 560px) {
      .top-links {
        top: 8px;
        width: min(100% - 20px, var(--content));
        align-items: stretch;
        flex-direction: column;
      }

      .creator-link-card { width: 100%; }
      .home-link { width: 100%; }
      .page { width: min(100% - 20px, var(--content)); padding-top: 145px; }
      .hero { padding: 22px; border-radius: 18px; }
      .section { margin-top: 12px; padding: 18px; border-radius: 14px; }
      .toc, .feature-grid { grid-template-columns: 1fr; }
      .subtitle { font-size: 16px; }
    }
  </style>
</head>
<body>
  <div class="top-links">
    <a class="home-link" href="https://sam.upfling.site/" title="Back to Sam's Character Map" aria-label="Back to Sam's Character Map">BACK TO Sam's CHARACTER MAP</a>

    <a
      class="creator-link-card"
      href="https://www.clank.world/@Samantha_"
      target="_blank"
      rel="noopener noreferrer"
      aria-label="Open Samantha_'s Clank profile"
    >
      <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAyCAYAAAAayliMAAAQ8ElEQVR4nG2aaY8b2XWGn3OrWBvJXth7t6RutUaWxuNxBmPHgA0HSJAgPzH/ID8gn/IlRhI4MbLAsSf2eBSNbY22lnohmztZ2735cG8trTEFdi2sqnvOe7b3nJIcP/0rwwcfY0AEjDFuW/9yZ9OcNYC4Hwx3Dt39rUMMBkHq49aD7Edax3cF+NZHffAIe7/Yu0WaY5HqyuqkW0+qh7dWl/bWPU9aW/eb1I8z9e3VoZjqWnNXAJHWheB/C07Mty2g7/x6Z0ELjrmLknGKfcsCFfKm+dmIe1T7r5NQG4y0z0qzb2oFWifdtlFSrFwtlIWWQMa07hZAY4w7o+88slmhklyBaIdypbxbuFLFiKBq0KQxS8tFfYt42wrmT2xaCjrEqzPaNNaI45AkigmikDgIUb7CUwoDaK0pi5J1lpGu1yxXK1arlMp6IhbxSjAjdkc3UNbIWQtay/ltxGszVaYzIGJaiLdC1f0Jo5DtzQ02Nnrk6ZLF7JbYMywmlyynQ44PdlnmwquXL/G8AM8PGewf8fj8lHVWMJvOGE0mZOsM82FcVIIZaw0x4k430ehXPt+G3FBJWEmrnfNotLYnwzBkb2+H7c0+6+kVL377r0wnE7Y2++w8PGe6nrCajVj3O8wXBdlqglI+ZVkyGb5iM8r45NMfcD1K2J1vcTudcX09JF1nTYB/4Pq1K1UxetcCYl2p8snauW3oYQzGWFPu7++yt7vL3lZIKEt+/sUXTCcTlFIsFmt+/ZsvUZ6Hr2A2nTGep2R5SdBRiAhaG5599RUnx/d4+tFjtDb88dUlW5sbXF+PuLy6qqLNol3n25arGEHE4BvzAfJVltHNvnH7QafDyckxe9tdnpwf4nvwb//yS8aTiY0FDYXW5HmOMZqg49GLfKbTqXuGuIwopGnKV89+x+HRMb1el4/ODjlapnwdCHEccXHxnjTLqKQX4+LOKWTEJgF1V0OX+02rlmh7IokTzs/PODve4rPvPqCbBMynY16/fEFRGMrSgPLqfG+MIc1yFqvUWc9QliUigohQlpqry0veXbxBRFCi6CURf/bkPqdHW5w/PKXbTWovqPAXl9rFCamMdrg7S9hjg0G7fU0Sx5yd3uP+YZ9Hpwd1bH3zx+es1xlZUaKNjaUojPB9vy6g82VKEvqIiKsr9mbP81guF1xcvCXLMpQISgRfeTw+PeTBwQanD+6RJDHGAWDqkHT7RqPqtHS3wIG2e2EQ8ODBPU4Oepwe7dQIGgNvvnnBKitcvEstnO938DyPjidEHY/j7Q0GvZAoDDDGoJRC65Ky1FxcXDAej0E1/q2U4sHxDg/2ezy4f48wCKjTrfMicWCoKgMZY1y20ZjSWgCjOTk5Yn874v7hwJlaEIT1asF4PCIrdL2o1pqiKFBKSKKIjX6X04MdfvD0EZ8+PKYbKtAlWpf1msPhDe/fXaC1RnnKAVQpscfhIObk+MhZQTtLN19VU6oqlrWreMZweLDPYDPm4b1dax2HPmKYjEes0rxFjSo/L8Bo4tBnd6PHyd4Oh/u7PDm9x9FmF180ZVkgolCiyLKC169fkaUpSokNI7eOiPDowQG7gy5Hhwc1NzJOFoxBWQPoxgpoMIY4idndGfDo/m5LcEGUvXkyvqXQxlpEBG0MuiwoihxT5vhoeqHPZhISdnz2d3c4PdpnZ6NL4CmHsuAr4eLtWxaLhSuUglLOEgpECU/ODtgZDIijqPEUR9Bqz6sqq4tv9nZ32O53CIMmAGusDazmc/JCo7U1bVmW9qG6QKGJfUXS8ehGAUHHJ0kStgcDkjgi7HQQoNPxEIHFYsnFxRubilEOKEFQeEqIo5CDQcTe/l6rEtjsqIzRDTewpIU4Dtnsdzk9GTRlu+Ybgtaa1XqNNhqt7bdCRSlFFHTY6nfxfY+g42PKkrcXbxkNr9lOIraTkMgTjNYYrNu9ffuGsihqaytRKAWiLJ/66PSIrY0uURw5b9E1J2woshiMGAbbA/qJx5/+GHSpWa/XGG1TWWXOKktgDLPlitliyXK5xPcUHdHkacbNZE6n02G7G5MEPoKtzKPhkOlsinaU2+UjLB+1x4N+xGBr6w63VHUJNlg2aGBjo8/+Tq+FeuNnxkBR5CxXK4uAajJH3Qg5N/OUx2yxRLwO6wLuHxzwF08f8dPzE54c7NDxPMpSI0pxOx5xfXWF0brxECcWrnqfHAzY2NxwHNXGq6MSDn2glySEPnTjqJXzG8ptjKEoS2azmcvJUmMVeD4bsc+9QZ/tXpf+Ro+r8YS3729YrlYcb20ThgWpEgojJOM52+KjlGI2nzMaDinyAi/yLPOsso5NO3S7EXHo0ev2mS8WiAi+rQ82bRoMcTchCr1asKp6SgMxeZ6RpmvXrVkaIUrjCRxv9vjOzjZBFOF3E7QIm/2YwXbPErwgIPAP+TgI+eQnf44uSzTC3/39P7BYzCnKktChXmUl4/4BdJOAJImYL2YYI46NIhgxiDYkUUwvCmrUa+Ebg5Jna9J1WndlHd/nx5895eNHpxxsb5LEIZ5LlU89DxHl3Kq80zxZgmk9Pk4ijC5ZLuck3cj6fs3LXENqDP0kIo5jqrO+rb6Ni3SCgCQJmkU+iANjYDGbYoz1Xa1LotDncG+H8/tH+J7neIv1ZW0MYkqngONZxhZD0U2z3et2WacpWZZaN/I7dr1WTy0I/SQkCAIwBo2gEEFclyNA0PGJw6AlcNv/bQDPZrPaIiJQlCXzdUpRNgLZDAVaG0pt64U2dZ64QwcEQ78bc3s7rFN0GzHTSjtRGNLp+G5tg6pZnrtQKYXvqz+BvEFrTZ5lLOZzfM+mP4CyLNHakOZ5raxUMxRjYfzAce4AA5DEEbP5nHcXF5RFefd34xioMfi+oJRny5blQq4dMFVr2Qjc3jbnNel6SVlqx2KtIp5SBJ3OtxQXaajBnWdXicM9vxdHZFnGH37/nOVy6Sq7qQWtOgJxvm97AkFVRazqxrTRFKWuEW+b2hjDfDalyFK0rj0QpYTr0ZjJbNEIaXntHbRNVUHd4EmUqgAmjkKKwnB5fcXzZ18ym07IspTSMVdck5mXmlKXjjgY/NoC7pIiL1mnGd04/IADOf+fjlkul5a8acednIDaVWTr11JnqSqf2z7QJkaRKj3aa5IkQjCURcG7izd0PMWj8zM2t3cojCIIIjCGdJ1RFqVNAkYqNmpqs2bpmsUyveMCYOc66XLOaj5hPp+jtZsJuU+W5XhVBvrgW5HEGu5KaW3quVIvjumGHfb7CVLm9EKPv/3rv+SHT8/YCUrS1QJEmK/WpJlrU6nYaONUrNI1y1V2x1+1thx+NZ/gIQSdgLwoKxUREVbr9I4L1fxErGVK55YfAiNOwTgK2eyGDDa7YDQbvkbNR0TFkm1Zsrh8yWIxZ7ZYs1qtakBsDLTY6HKxZLm+mwWM0US+4uPvfso6y5kv5hQOvcqnS20IO52W4qCNtozTVNyFO27T/NN04witIQpjunGML4bf/PI/+cPvvybLC9azEe/eXTBfpSyXy/p5qpoUV9/ZfEZhFHlR2AuUotfrEycbfPHrX/H6zVvSLMOr0qT7rLMMrctW4FdbNwbBWlKXrkA49JUuUVrjUfL5x4+IwpCyLPE8H20Uyg8QL2CV5jx//jVpBrPZvDK+64mrr6PK48mYy+uZpQmdgOUy5Wc/+yf+47/+m9HomnWWU5oqn1s007xgOJ6idemEr0GydaA94XZuo9OUcrmELENlGT/65DG9bozveXgCeW7Rvp1MeXczYrLIGU/G6LKkIku+TRVVH2y349GY6dYGyvNYrVJevnrFaDRiuZxTFCVZoTGoO7UiTTOSKLTuUiFfDZlagleZrTSa9WKOLkvyxZLV6j0P9vc43ttlulgQhz7GGLI858Wb93zzfsjO6Wfcjm6bmWnFRt3EHsGafTKdMJuvuLyZ0k8iXn7zkvHE5uWiKMhyjecptLbDX5dX0Os1xTq1pvU8xPNrC31YyESB3+1SFgVlVpClGS/fvef4+B5G53iutxhOpvz8i2esiemECdPZ28bfjbFzoWaaqsAVp6ura97dTBiORkxmM3SZYwysM402YhUWhS4hiWL+5oef8r2zIzbDiF6ng1eW4KrpHeFdjFAN1PKCw61Nnp6dknT7dAOf7e0Bt7MlqzTlH3/xK95cjbl39pSry0va86faAkaqjONyhBEm0wm3o00mo4z5bEKkoMhzikI3faunODo84Efffcj3zw74+uVrbiZz+mGHk90d4q1tVGCVtW5V1ilUa8NquSQwhsntiM2dPU7PzyiKktHFDX+4WvL+d9/wzbshe0fnaCNMpxNAubGK9U6/4hfWBNq+D3DBefHuLY8fP8ELYtLlNXmpMaLqUYbneTw8O+Vwu89qtWZNh/OzMzxdME/XkKeEvu/eMTT1HiDPMtazOZ0kIuxvMF+tWN2OGS4Lvnp9w+vhjNcXQ6Jki9OPPuHrr/8PxPbHbafxkeb9QOPN9oI8t0On8+98xvNf/zOFFpSSurh1goDr4Zj/zVd8//yE8+N9OiKUeUqn42GUchmpNQxxQbxerFgvV8RhyCIreXUz483NhPc3E+brlLLUeH7E9z7/KW9ev6bIC6rptsG+7DAiyP75j02V85qy35A4sFOKncEWX/7qF+TZoh6lbA8G9Hs9yqKkl0Q8Pt7l8ycPiD1YzKdk4hHGSSO8svGljWE8umU6umVRGF7crBjPVxStgDfis3vyiNFwzGg0qjOPiKqBAKfAXcGh6tJqyothZzBgf3+f3/7Pv7OYDTk6PuHw6ISbq3d2LuTcoxfHfH62w+lOn1QpwjjB8zwrvLNzqTWL+ZLLtxc8e3PF+1lJJ4wAWzhRIUenT7l8/57haOgIoKrbT0t7nCIH5z82FZ2ohLZs0nZRIqYeXG1sbHL//inj4Tse3DtgNpvy4vdfOdQUnuehtcYTw6O9PmGgyFHEgUcSdMjznLIsWaYZL94PmczWrPOCQgv9fp8gCAm6Aw7vPeL1q1dMZ2MEN59S7h1Z3aPbfd+5PxUrrcqyo+xuKxgU0+mE58+fcXxywjyFdVZRB1DKKYphuVrz7OWM0WTGKs3Y7AZ8fH+foiiIw4DpMuXl22vyEsIwIowiVCdh5+ghiM/zZ8/Ii6xp7KXl8zTdnWDw22/87OsCYxtaZUd/tRKAEUWeZ7z85gVbWwMODg7YO3nK8Po1xXruaISxr1AXC4qyBAFtBM/zSMIO/V6XRX6LQfA8xcbggN3DU/qbAy4v3zO+HTlwvTrXC1JvpU5B9thv/L8aIrWhh+Z9qzOO2Mxyezvk9vaGra0d9u89JgpCbq7eMpsOUX6O+Fn9AtsApRF63T6ZirjJ1hw//D5HJw/JiozRcMjFxZcuvwsYRZW4GsTr135WHHEBf/DoJ6aezrXiwHpUdb4qcPqD881x0OnQ39ii10uIooQwDDGALm1j6Xk26LI0ZbVaslgsmE7G5EVOU5la/v1BxbVu7KqAVLMswW96jEo/qf87QGWIpgQpanJmoKqKUJLlOcPhNcNhMxzwfTfhc8AURdHCtBGucgdzh9Y09aguf0Ys22lNOXwl4mhJI7GgWko4kufIXrOw2xgweG4VXTNaATu4bVSuc3jlw3ZP1WuLE1BM6/fWWN/GtLjW2iqhqsFSC1bu/ieO6kW3tY+heWB9Z72IcvlZYVzZN20BEGrCWO9X3t1A3SrcjtZU/bTU1jXONZp+oPpIDS5KGvehahkqQauVxMaHTXOmKTR33KSlA02CqBBXdcS2KYeqSb7Vs3WNixkx8P/2q9s2rXfFlQAAAABJRU5ErkJggg==" alt="Samantha_ avatar">
      <span class="creator-link-meta">
        <span class="creator-link-kicker">AUTHOR</span>
        <span class="creator-link-name">Sam · @Samantha_ on Clank</span>
        <span class="creator-link-url">https://www.clank.world/@Samantha_</span>
      </span>
    </a>
  </div>

  <main class="page">
    <header class="hero">
      <div class="eyebrow">Chrome Extension Guide</div>
      <h1>Clank Scene Indexer — Scene &amp; Popularity Collector</h1>
      <div class="creator-credit">
        <span>Created by <a href="https://www.clank.world/@Samantha_" target="_blank" rel="noopener noreferrer">Samantha_</a></span>
        <a class="back-home-inline" href="https://sam.upfling.site/">BACK TO Sam's CHARACTER MAP</a>
      </div>
      <p class="subtitle">
        A creator-side utility for discovering, collecting, organizing, and comparing scenes attached to your own Clank characters.
      </p>
      <div class="badges">
        <span class="badge beta">Beta / Testing Build</span>
        <span class="badge">Scene Discovery</span>
        <span class="badge">Popularity Sorting</span>
        <span class="badge">Public / Private Detection</span>
      </div>

      <nav class="toc" aria-label="Page navigation">
        <a href="#about">What It Does</a>
        <a href="#install">Installation</a>
        <a href="#use">How to Use</a>
        <a href="#testing">Testing Notes</a>
      </nav>
    </header>

    <section class="section" id="about">
      <h2>What This Extension Does</h2>
      <p>
        <strong>Clank Scene Indexer</strong> helps Clank creators find and organize scenes belonging to their own characters. It scans the characters available to your signed-in account, lets you choose which characters to process, and collects useful information about each scene.
      </p>

      <div class="feature-grid">
        <div class="feature"><b>Scene details</b>Scene title, scene URL, title image URL, and public/private status.</div>
        <div class="feature"><b>Popularity data</b>Visible scene message counts such as <code>62.6K</code>, <code>172K</code>, or <code>1.2M</code>.</div>
        <div class="feature"><b>Overall scene ranking</b>Sort all collected scenes together by message count, regardless of character.</div>
        <div class="feature"><b>Character popularity totals</b>Add together the known message counts from all collected scenes belonging to each character.</div>
      </div>

      <div class="callout warning">
        <strong>Please understand that this extension is still in active testing.</strong><br>
        Clank is a live website and its page structure can change. Some characters, scenes, layouts, or newly changed pages may still behave unexpectedly. If something fails, please report the character and what happened so the scanner can be improved.
      </div>

      <div class="callout private">
        <strong>Private-scene support does not bypass Clank permissions.</strong><br>
        The extension can only collect private scenes already visible to the Clank account you are currently signed into.
      </div>
    </section>

    <section class="section" id="install">
      <h2>Installation</h2>
      <p>The extension is currently installed manually as an <strong>unpacked Chrome extension</strong>.</p>

      <div class="steps">
        <article class="step">
          <h3><span class="step-number">1</span> Download and unzip the extension</h3>
          <p>Download the extension ZIP from GitHub and extract it somewhere you will not accidentally delete it.</p>
          <a class="download-link" href="https://github.com/samanthatsang3/clank_scene_extractor/archive/refs/heads/main.zip">DOWNLOAD CLANK SCENE INDEXER FROM GITHUB</a>
          <p>Direct download: <a href="https://github.com/samanthatsang3/clank_scene_extractor/archive/refs/heads/main.zip">https://github.com/samanthatsang3/clank_scene_extractor/archive/refs/heads/main.zip</a></p>
          <p>The extracted folder should contain files such as:</p>
          <pre>manifest.json
collector-main.js
popup.html
popup.js
popup.css
README.md</pre>
          <p><strong>Do not select the ZIP itself when installing.</strong> Chrome needs the extracted folder that directly contains <code>manifest.json</code>.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/4c105b46b77a.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/4c105b46b77a.png" alt="Extracted extension folder" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">2</span> Open Chrome Extensions</h3>
          <p>Enter the following into Chrome's address bar:</p>
          <pre>chrome://extensions</pre>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/cf4d602753dd.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/cf4d602753dd.png" alt="Chrome Extensions page" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">3</span> Enable Developer Mode</h3>
          <p>Turn on <strong>Developer mode</strong> in the upper-right corner of the Extensions page. This reveals additional controls including <strong>Load unpacked</strong>.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/6a3225242c57.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/6a3225242c57.png" alt="Developer mode enabled" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">4</span> Load the unpacked extension</h3>
          <p>Click <strong>Load unpacked</strong> and choose the extracted extension folder — the folder that directly contains <code>manifest.json</code>.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/6650df49d2e3.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/6650df49d2e3.png" alt="Extension successfully installed" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">5</span> Pin the extension</h3>
          <p>Open Chrome's Extensions / puzzle-piece menu and pin <strong>Clank Scene Indexer</strong> so it is easy to access.</p>
        </article>
      </div>
    </section>

    <section class="section" id="use">
      <h2>How to Use</h2>

      <div class="steps">
        <article class="step">
          <h3><span class="step-number">1</span> Sign into Clank and open your creator profile</h3>
          <p>Make sure you are already signed into the Clank account whose characters you want to scan.</p>
          <p>Then open your <strong>creator profile page</strong>, for example:</p>
          <pre>https://www.clank.world/@YourUsername</pre>
          <p>Launch the extension from your <strong>user profile</strong>, not from an individual character page.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/b04cc17b84c9.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/b04cc17b84c9.png" alt="Your Clank creator profile" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">2</span> Launch the collector</h3>
          <p>Click the <strong>Clank Scene Indexer</strong> icon in Chrome, then click <strong>Launch Collector</strong>.</p>
          <p>You can close the small Chrome extension popup after launching it. The actual collector interface appears directly on your Clank page.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/2ce81c403a47.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/2ce81c403a47.png" alt="Extension popup" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">3</span> Discover your characters</h3>
          <p>In the floating collector panel, click <strong>Discover Characters</strong>.</p>
          <p>The collector opens a separate Clank worker window and visits:</p>
          <pre>https://www.clank.world/your-characters</pre>
          <p>It automatically scrolls through the page and discovers the characters belonging to your account.</p>
          <div class="callout warning"><strong>Do not close the worker window while it is scanning.</strong></div>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/0fb261e8b22a.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/0fb261e8b22a.png" alt="Discover Characters button" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">4</span> Choose which characters to scan</h3>
          <p>After discovery finishes, the collector pauses and gives you a selectable list of your characters.</p>
          <ul>
            <li>Select All</li>
            <li>Select None</li>
            <li>Invert Selection</li>
            <li>Search / filter characters</li>
            <li>Scan only the characters you want</li>
          </ul>
          <p>Each character also has an optional <strong>Allow hidden/private scenes</strong> setting.</p>
          <div class="callout private">
            Private scenes are disabled by default. Enabling this option only allows the collector to include private scenes already accessible to your signed-in account.
          </div>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/fd55d73d2531.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/fd55d73d2531.png" alt="Character selection" loading="lazy">
            </a>
          </div>
        </article>

        <article class="step">
          <h3><span class="step-number">5</span> Scan the selected characters</h3>
          <p>Click <strong>Scan Selected Characters</strong>. The worker window begins searching Clank for each selected character.</p>
          <p>The collector checks that scenes belong to the correct creator and character before adding them to the results.</p>
          <pre>Character
Scene title
Public / Private status
Scene message count
Scene URL
Scene title image URL</pre>
          <p>Scanning may take some time when many characters are selected. Leave the worker window alone until the scan completes.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/a99b42fe9a87.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/a99b42fe9a87.png" alt="Scan progress" loading="lazy">
            </a>
          </div>
        </article>
      </div>
    </section>

    <section class="section" id="results">
      <h2>Viewing Results</h2>
      <p>When the scan finishes, the collector displays the scenes it found along with visibility, links, images, and message counts.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/369bab668222.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/369bab668222.png" alt="Normal collected results" loading="lazy">
            </a>
          </div>

      <h3>Most Popular Scenes Overall</h3>
      <p>This option creates one global leaderboard across <strong>every collected scene</strong>, regardless of which character it belongs to.</p>
      <div class="leaderboard">#1 Character A — Scene Name — 172K
#2 Character B — Scene Name — 96.4K
#3 Character C — Scene Name — 62.6K
#4 Character A — Another Scene — 47K</div>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/d6be631e4deb.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/d6be631e4deb.png" alt="Sort menu" loading="lazy">
            </a>
          </div>

      <h3>Least Popular Scenes Overall</h3>
      <p>Uses the same overall scene list, but places the lowest known message counts first.</p>

      <h3>Most Popular Characters</h3>
      <p>The collector can also add together the known message counts from all collected scenes belonging to each character.</p>
      <div class="leaderboard">1. Character A — 410.8K total scene messages
2. Character B — 305.2K total scene messages
3. Character C — 194K total scene messages</div>
      <p>Because Clank displays abbreviated counts such as <code>62.6K</code> and <code>1.2M</code>, combined character totals should be treated as <strong>approximate totals based on the displayed values</strong>.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/1b52f4747084.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/1b52f4747084.png" alt="Character popularity leaderboard" loading="lazy">
            </a>
          </div>
    </section>

    <section class="section" id="export">
      <h2>Copying and Exporting Results</h2>
      <p>The collector can export the information it finds. Depending on the selected sort mode, exported results follow the same ordering shown in the collector.</p>
      <ul>
        <li><strong>Copy Results</strong></li>
        <li><strong>Export JSON</strong></li>
        <li><strong>Export CSV</strong></li>
      </ul>
      <p>Popularity information is included where available.</p>
          <div class="screenshot image-filled">
            <a href="https://cdn.imgchest.com/files/369bab668222.png" target="_blank" rel="noopener noreferrer">
              <img src="https://cdn.imgchest.com/files/369bab668222.png" alt="Export controls" loading="lazy">
            </a>
          </div>
    </section>

    <section class="section" id="testing">
      <h2>Important Testing Notes</h2>
      <div class="callout warning">
        <strong>This is currently a Beta / testing build.</strong>
      </div>
      <ul>
        <li>Clank is a live website and its interface may change.</li>
        <li>A Clank layout change can temporarily break part of the scanner.</li>
        <li>Scene counts displayed by Clank are abbreviated and rounded.</li>
        <li>Private scenes can only be collected when your signed-in account can already access them.</li>
        <li>Unicode/native-script character names are supported, but unusual Clank rendering changes may still require additional testing.</li>
        <li>Do not close the worker window while a scan is actively running.</li>
      </ul>

      <h3>What to include in a bug report</h3>
      <p>If something fails, please include:</p>
      <ul>
        <li>Character name</li>
        <li>Character URL</li>
        <li>Expected number of scenes</li>
        <li>Number of scenes actually found</li>
        <li>Any visible error or status message</li>
      </ul>
    </section>

    <footer class="footer">
      Clank Scene Indexer — Beta / Testing Build<br>
      Created by <a href="https://www.clank.world/@Samantha_" target="_blank" rel="noopener noreferrer">Samantha_</a> • <a href="https://sam.upfling.site/">BACK TO Sam's CHARACTER MAP</a><br>
      This tool is intended to organize information already available to your signed-in Clank account.
    </footer>
  </main>
</body>
</html>
