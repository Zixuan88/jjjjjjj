import os
import sys
from datetime import datetime, timezone, timedelta
import requests

# ========== 設定區 ==========
# 指定地點（台北市中心，可自行修改）
LATITUDE = 25.0330
LONGITUDE = 121.5654
LOCATION_NAME = "台北市"

# 門檻值
RAIN_THRESHOLD = 60          # %
TEMP_THRESHOLD = 33          # °C
AQI_THRESHOLD = 100

# Telegram（從環境變數讀取，禁止寫死）
TELEGRAM_BOT_TOKEN = os.getenv("TELEGRAM_BOT_TOKEN")
TELEGRAM_CHAT_ID = os.getenv("TELEGRAM_CHAT_ID")

# ========== API 函式 ==========
def get_weather_data():
    """取得今日最高溫度與最高降雨機率（Open-Meteo，免 API Key）"""
    url = "https://api.open-meteo.com/v1/forecast"
    params = {
        "latitude": LATITUDE,
        "longitude": LONGITUDE,
        "daily": "temperature_2m_max,precipitation_probability_max",
        "timezone": "Asia/Taipei",
        "forecast_days": 1
    }
    resp = requests.get(url, params=params, timeout=15)
    if resp.status_code != 200:
        raise Exception(f"天氣 API 錯誤 HTTP {resp.status_code}: {resp.text}")

    data = resp.json()
    daily = data["daily"]
    max_temp = daily["temperature_2m_max"][0]
    max_rain_prob = daily["precipitation_probability_max"][0]
    return max_temp, max_rain_prob

def get_aqi_data():
    """取得目前 AQI（Open-Meteo Air Quality，免 API Key）"""
    url = "https://air-quality-api.open-meteo.com/v1/air-quality"
    params = {
        "latitude": LATITUDE,
        "longitude": LONGITUDE,
        "hourly": "us_aqi",
        "timezone": "Asia/Taipei",
        "forecast_days": 1
    }
    resp = requests.get(url, params=params, timeout=15)
    if resp.status_code != 200:
        raise Exception(f"空氣品質 API 錯誤 HTTP {resp.status_code}: {resp.text}")

    data = resp.json()
    # 取最近一小時的 US AQI
    aqi_list = data["hourly"]["us_aqi"]
    # 過濾掉 None
    valid_aqi = [v for v in aqi_list if v is not None]
    if not valid_aqi:
        raise Exception("空氣品質資料為空")
    current_aqi = valid_aqi[0]
    return current_aqi

def generate_advice(max_temp, max_rain_prob, aqi):
    """根據多個條件產生通勤建議（可同時成立）"""
    advices = []

    if max_rain_prob >= RAIN_THRESHOLD:
        advices.append(f"☔ 降雨機率達 {max_rain_prob}%（≥{RAIN_THRESHOLD}%），請記得攜帶雨傘！")

    if max_temp >= TEMP_THRESHOLD:
        advices.append(f"🌡️ 最高溫度達 {max_temp}°C（≥{TEMP_THRESHOLD}°C），請做好防曬並補充水分！")

    if aqi >= AQI_THRESHOLD:
        advices.append(f"😷 空氣品質 AQI 達 {aqi}（≥{AQI_THRESHOLD}），建議配戴口罩！")

    if not advices:
        advices.append("✅ 目前天氣與空氣品質皆正常，適合外出通勤！")

    return advices

def send_telegram(message: str):
    """透過 Telegram Bot 傳送通知"""
    if not TELEGRAM_BOT_TOKEN or not TELEGRAM_CHAT_ID:
        raise Exception("缺少環境變數 TELEGRAM_BOT_TOKEN 或 TELEGRAM_CHAT_ID")

    url = f"https://api.telegram.org/bot{TELEGRAM_BOT_TOKEN}/sendMessage"
    payload = {
        "chat_id": TELEGRAM_CHAT_ID,
        "text": message,
        "parse_mode": "HTML"
    }
    resp = requests.post(url, json=payload, timeout=15)
    if resp.status_code != 200:
        raise Exception(f"Telegram 發送失敗 HTTP {resp.status_code}: {resp.text}")
    print("Telegram 通知已成功發送")

def main():
    print("=" * 50)
    print("智慧通勤風險通知系統 開始執行")
    print(f"時間：{datetime.now(timezone(timedelta(hours=8))).strftime('%Y-%m-%d %H:%M:%S')} (台灣)")
    print("=" * 50)

    try:
        # 1. 取得天氣資料
        max_temp, max_rain_prob = get_weather_data()
        print(f"最高溫度：{max_temp}°C")
        print(f"最高降雨機率：{max_rain_prob}%")

        # 2. 取得空氣品質
        aqi = get_aqi_data()
        print(f"目前 AQI：{aqi}")

        # 3. 產生建議
        advices = generate_advice(max_temp, max_rain_prob, aqi)

        # 4. 組合訊息
        message = (
            f"<b>🚌 智慧通勤風險通知</b>\n"
            f"📍 地點：{LOCATION_NAME}\n"
            f"🕐 時間：{datetime.now(timezone(timedelta(hours=8))).strftime('%Y-%m-%d %H:%M')}\n"
            f"────────────────\n"
            f"🌡️ 最高溫度：<b>{max_temp}°C</b>\n"
            f"🌧️ 最高降雨機率：<b>{max_rain_prob}%</b>\n"
            f"🌫️ 空氣品質 AQI：<b>{aqi}</b>\n"
            f"────────────────\n"
            f"<b>通勤建議：</b>\n"
        )
        for advice in advices:
            message += f"• {advice}\n"

        print("\n產生的通知內容：")
        print(message)

        # 5. 傳送 Telegram
        send_telegram(message)
        print("執行完成 ✅")

    except Exception as e:
        error_msg = f"❌ 系統錯誤：{str(e)}"
        print(error_msg)
        # 嘗試把錯誤也傳給 Telegram（方便排查）
        try:
            if TELEGRAM_BOT_TOKEN and TELEGRAM_CHAT_ID:
                send_telegram(error_msg)
        except:
            pass
        sys.exit(1)

if __name__ == "__main__":
    main()
