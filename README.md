import tkinter as tk
import yfinance as yf
import time

class TradingBot:
    def __init__(self):
        self.root = tk.Tk()
        self.root.title("ربات معامله‌گر")
        self.root.geometry("300x150")
        
        self.label = tk.Label(self.root, text="وضعیت: آماده", font=('Arial', 12))
        self.label.pack(pady=10)
        
        self.start_btn = tk.Button(self.root, text="شروع", command=self.start_bot, bg='green', fg='white')
        self.start_btn.pack(pady=5)
        
        self.stop_btn = tk.Button(self.root, text="توقف", command=self.stop_bot, bg='red', fg='white')
        self.stop_btn.pack(pady=5)
        
        self.running = False
        self.symbol = "BTC-USD"

    def get_signal(self):
        try:
            data = yf.Ticker(self.symbol).history(period="1d", interval="15m")
            sma_20 = data['Close'].rolling(20).mean().iloc[-1]
            current_price = data['Close'].iloc[-1]
            return "خرید" if current_price > sma_20 else "فروش"
        except Exception as e:
            return f"خطا: {e}"

    def start_bot(self):
        self.running = True
        self.label.config(text="وضعیت: در حال اجرا")
        while self.running:
            signal = self.get_signal()
            self.label.config(text=f"سیگنال: {signal}")
            self.root.update()
            time.sleep(60)

    def stop_bot(self):
        self.running = False
        self.label.config(text="وضعیت: متوقف شد")

    def run(self):
        self.root.mainloop()

if __name__ == "__main__":
    bot = TradingBot()
    bot.run()