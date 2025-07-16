# 医学手术复盘AI Agent - 产品需求文档

## 需求分析
1、后端要求：对接cozeAPI，并把例程的配置单独放到配置文件中，以便修改，使用Json文件进行API的配置管理；
2、前端要求：现在的前端页面已经实现，分别在index、upload、analysis与ai四个文件夹中，请你把实现好的后端与ai对话页面对接

## 后端例程如下：
现在有一个coze例程如下：

"""
This example is about how to use the streaming interface to start a chat request
and handle chat events
"""

import os
# Our official coze sdk for Python [cozepy](https://github.com/coze-dev/coze-py)
from cozepy import COZE_CN_BASE_URL

# Get an access_token through personal access token or oauth.
coze_api_token = 'pat_BabxcQ99cMx3ziABhpRshfsvXBP0HXgke1wVigzquPehrOshql7Fr61kXhex1S4b'
# The default access is api.coze.com, but if you need to access api.coze.cn,
# please use base_url to configure the api endpoint to access
coze_api_base = COZE_CN_BASE_URL

from cozepy import Coze, TokenAuth, Message, ChatStatus, MessageContentType, ChatEventType  # noqa

# Init the Coze client through the access_token.
coze = Coze(auth=TokenAuth(token=coze_api_token), base_url=coze_api_base)

# Create a bot instance in Coze, copy the last number from the web link as the bot's ID.
bot_id = '7519906707460898851'
# The user id identifies the identity of a user. Developers can use a custom business ID
# or a random string.
user_id = '123456789'

# Call the coze.chat.stream method to create a chat. The create method is a streaming
# chat and will return a Chat Iterator. Developers should iterate the iterator to get
# chat event and handle them.
for event in coze.chat.stream(
    bot_id=bot_id,
    user_id=user_id,
    additional_messages=[
        Message.build_user_question_text("Tell a 500-word story."),
    ],
):
    if event.event == ChatEventType.CONVERSATION_MESSAGE_DELTA:
        print(event.message.content, end="", flush=True)

    if event.event == ChatEventType.CONVERSATION_CHAT_COMPLETED:
        print()
        print("token usage:", event.chat.usage.token_count)

        