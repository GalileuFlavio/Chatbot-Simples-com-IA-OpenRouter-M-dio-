# Chatbot-Simples-com-IA-OpenRouter-M-dio-
Preenchimento: adicione sua API key na linha 15

"""
Chatbot usando OpenRouter (acesso gratuito a modelos de IA)
Preenchimento: adicione sua API key na linha 15
"""
import requests

class ChatBot:
    def __init__(self):
        # ← PREENCHA AQUI: obtenha em https://openrouter.ai/ (grátis)
        self.api_key = "SUA_API_KEY_AQUI"
        self.url = "https://openrouter.ai/api/v1/chat/completions"
        self.historico = []
    
    def perguntar(self, mensagem_usuario):
        """Envia pergunta e retorna resposta da IA"""
        self.historico.append({"role": "user", "content": mensagem_usuario})
        
        headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
        
        dados = {
            "model": "openai/gpt-3.5-turbo",  # Modelo gratuito disponível
            "messages": self.historico
        }
        
        try:
            resposta = requests.post(self.url, json=dados, headers=headers)
            resposta_json = resposta.json()
            
            if "choices" in resposta_json:
                mensagem_bot = resposta_json["choices"][0]["message"]["content"]
                self.historico.append({"role": "assistant", "content": mensagem_bot})
                return mensagem_bot
            else:
                return f"Erro: {resposta_json.get('error', 'Desconhecido')}"
                
        except Exception as e:
            return f"Erro de conexão: {e}"

# === COMO CONFIGURAR ===
# 1. Acesse https://openrouter.ai/ e crie conta (grátis)
# 2. Gere uma API Key e cole acima
# 3. Execute: python chatbot.py
# 4. Converse digitando suas mensagens (digite 'sair' para encerrar)

if __name__ == "__main__":
    print("🤖 ChatBot IA (digite 'sair' para encerrar)")
    print("=" * 50)
    
    bot = ChatBot()
    
    while True:
        usuario = input("\nVocê: ")
        if usuario.lower() == 'sair':
            break
        
        resposta = bot.perguntar(usuario)
        print(f"Bot: {resposta}")
