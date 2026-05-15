export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { input, servings } = req.body;
  if (!input) return res.status(400).json({ error: 'input is required' });

  const apiKey = process.env.GEMINI_API_KEY;
  if (!apiKey) return res.status(500).json({ error: 'API key not configured' });

  const prompt = `あなたは料理の専門家です。ユーザーが伝えた食材・気分・要望をもとに、ひとつの美味しいレシピを提案してください。

【重要】材料の分量と人数は必ず「${servings || '2人分'}」で計算してください。servingsフィールドにも必ず「${servings || '2人分'}」を入れてください。

必ず以下のJSON形式のみで返してください。前置き・説明・マークダウン記号は一切不要です。

{
  "emoji": "料理を表す絵文字1文字",
  "name": "料理名",
  "subtitle": "一言キャッチコピー（20文字以内）",
  "time": "調理時間（例：約20分）",
  "difficulty": "難易度（かんたん／ふつう／本格 のいずれか）",
  "servings": "${servings || '2人分'}",
  "ingredients": ["食材1（量）", "食材2（量）"],
  "steps": ["手順1", "手順2", "手順3"],
  "tip": "おいしく作るコツ（1〜2文）"
}

ユーザーの入力：${input}`;

  const maxRetries = 3;
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      if (attempt > 1) await new Promise(r => setTimeout(r, 2000 * attempt));
      const geminiRes = await fetch(
        `https://generativelanguage.googleapis.com/v1/models/gemini-2.5-flash-lite:generateContent?key=${apiKey}`,
        {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            contents: [{ parts: [{ text: prompt }] }],
            generationConfig: { temperature: 0.8, maxOutputTokens: 2048 }
          })
        }
      );
      const data = await geminiRes.json();
      if (geminiRes.status === 503 || geminiRes.status === 429) {
        if (attempt < maxRetries) continue;
        return res.status(503).json({ error: 'サーバーが混雑しています。しばらく待ってからお試しください。' });
      }
      if (!geminiRes.ok) return res.status(geminiRes.status).json({ error: data.error?.message || 'API error' });
      if (data.error) return res.status(500).json({ error: data.error.message });

      const text = data.candidates?.[0]?.content?.parts?.[0]?.text || '';
      const clean = text.replace(/```json|```/g, '').trim();
      const jsonMatch = clean.match(/\{[\s\S]*\}/);
      const recipe = JSON.parse(jsonMatch ? jsonMatch[0] : clean);
      return res.status(200).json({ recipe });
    } catch(e) {
      if (attempt < maxRetries) continue;
      return res.status(500).json({ error: e.message });
    }
  }
}
