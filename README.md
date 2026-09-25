# Botchat_ai
import os
from flask import Flask, request, jsonify
from openai import OpenAI

app = Flask(__name__)

client = OpenAI(
    api_key=os.environ.get("OPENAI_API_KEY")
)

SYSTEM_PROMPT = """
أنت مساعد ذكاء اصطناعي ودود وذكي.
افهم لغة المستخدم ورد بنفس اللغة التي يستخدمها.
إذا تحدث المستخدم بالدارجة الجزائرية، يمكنك الرد بالدارجة الجزائرية.
كن واضحًا ومختصرًا وطبيعيًا.
لا تخترع معلومات غير متأكد منها.
"""

@app.route("/")
def home():
    return "AI Bot is running!"

@app.route("/chat", methods=["POST"])
def chat():
    data = request.get_json()
    message = data.get("message", "")

    if not message:
        return jsonify({"error": "No message"}), 400

    response = client.responses.create(
        model="gpt-5.6-luna",
        instructions=SYSTEM_PROMPT,
        input=message
    )

    return jsonify({
        "reply": response.output_text
    })

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=10000)
