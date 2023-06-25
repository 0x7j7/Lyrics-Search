# Lyrics-Search
一个简单的Python程序，通过与Genius API交互，允许用户根据歌曲名搜索并获取歌词。该代码库提供了一个快速方便的方法，使用户能够以编程方式访问歌曲歌词，用于各种用途，如歌词分析、歌词展示等。用户只需提供歌曲名，代码将使用Genius API返回与输入匹配的歌词文本。
import requests

def get_lyrics(song_name):
    api_key = 'YOUR_API_KEY'  # 在Genius网站上注册并获取API密钥
    base_url = 'https://api.genius.com'
    headers = {'Authorization': 'Bearer ' + api_key}
    search_url = base_url + '/search?q=' + song_name

    response = requests.get(search_url, headers=headers)
    data = response.json()

    # 检查API响应
    if 'response' in data:
        hits = data['response']['hits']
        if hits:
            # 获取第一个结果的歌词URL
            song_url = hits[0]['result']['url']

            # 使用歌词URL获取歌词文本
            lyrics_response = requests.get(song_url)
            lyrics_data = lyrics_response.text

            return lyrics_data
        else:
            return "没有找到该歌曲的歌词。"
    else:
        return "请求失败，请检查API密钥和网络连接。"

# 测试代码
song_name = input("请输入歌曲名：")
lyrics = get_lyrics(song_name)
print(lyrics)
