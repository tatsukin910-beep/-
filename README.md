# -
たっちゃんメンタル
エコー "# -" >> README.md
git init
git add README.md
git commit -m "最初のコミット"
git ブランチ -M メイン
git リモートでオリジンを追加 https://github.com/tatsukin910-beep/-.git
git push -u origin mainimport streamlit as st
import random

# クラス定義（v4のエッセンスを抽出）
class CognitiveOS:
    def __init__(self, stress=0.3):
        self.stress = stress
        self.completion = 0.0
        self.loops = 0

    def process(self, event):
        self.completion = 0.0
        self.loops = 0
        logs = []

        while self.completion < 0.5:
            self.loops += 1
            risk = self.calc_risk(event)
            
            # 納得度の更新ロジック
            if risk < 0.3:
                self.completion += 0.3
            elif risk > 0.6:
                self.completion *= 0.5 # リスクが高いと納得度が下がる
            
            logs.append(f"Loop {self.loops}: Risk={risk:.2f}, Completion={self.completion:.2f}")

            # 離脱条件
            if risk > 0.85 or self.loops >= 3:
                return "⚠️ EMERGENCY → 今は決めない", logs

        return "✅ OK → ソフト判断で進む", logs

    def calc_risk(self, event):
        base = 0.65 if any(x in event for x in ["気持ち", "返信", "本音"]) else 0.3
        risk = base * (1 + self.stress * 0.6)
        risk *= (1 + self.loops * 0.1)
        return min(1.0, risk + random.uniform(-0.05, 0.05))

# --- UI部 ---
st.set_page_config(page_title="Cognitive OS", page_icon="🧠")
st.title("🧠 Cognitive OS v4")

# サイドバーで内部パラメータを調整
st.sidebar.header("内部ステータス")
user_stress = st.sidebar.slider("現在のストレスレベル", 0.0, 1.0, 0.3)

# メイン入力
event = st.text_input("何が起きていますか？（イベント入力）", placeholder="例：返信が来ない、自分の気持ちがわからない")

if st.button("▶ 思考プロセス実行"):
    os = CognitiveOS(stress=user_stress)
    result, logs = os.process(event)
    
    # 結果表示
    if "EMERGENCY" in result:
        st.error(result)
        st.warning("推奨アクション: **スマホを閉じる / 物理的に離れる / 寝る**")
    else:
        st.success(result)
    
    # 思考のログを折りたたみで表示
    with st.expander("思考ログを確認"):
        for log in logs:
            st.text(log)

# クイックボタン
col1, col2 = st.columns(2)
with col1:
    if st.button("🛑 強制停止（Panic Button）"):
        st.info("🚨 DO NOT DECIDE NOW. 別のことをしてください。")
with col2:
    if st.button("🔄 状態リセット"):
        st.rerun()

