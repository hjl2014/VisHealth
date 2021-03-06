# 获取Jawbone UP中的个人数据（二）非官方API #

# 2. 用户行为概况 #

## 用户概况 ##

打开 UP 的 Home 页，首先会展示用户当天的任务完成情况，以及用户的历史行为。下面我们就来讨论，这个页面使用到两个API **users/%userXid%/score** 和 **users/%userXid%/social** 。

请注意，Eric 提到的 **users/%userXid%/healthCredits** API 似乎已经**不再可用**。

![Home页](imgs/jawbone_up_1.png)

在使用 users/%userXid%/... 类型的 API 时，需要注意的其中 %userXid% 的取值可以有两种形式：

- @me ，用于访问自己的信息
- User XID 值 ，可以使用 login 返回的 "user"."xid" 访问自己的信息。或者可以利用从朋友查找中获取的 User XID值，查询他人的信息（推测，未实验）。

### users/%userXid%/score ###

**users/%userXid%/score** 用于查看用户的运动完成情况，显示当天用户运动、睡眠、饮食等综合信息。

**Request：**

	GET
	https://jawbone.com/nudge/api/users/@me/score	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/score
	https://jawbone.com/nudge/api/users/@me/score?date=20130609		#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/score?date=20130609

**Params：**
	
    'date' : datestr #格式为yyyymmdd，如果没有 date 参数，返回今日数据

**Return :** 

返回信息包括 Mood、Move、Sleep、Meals 等按日统计信息，这个数据还包括了当用户点击各种行为的状态条时，打开具体行为统计页面的信息：

![Score](imgs/jawbone_up_2.png)

完整JSON示例如下：

	{
	    "meta": {
	        "code": 200, 
	        "message": "OK", 
	        "user_xid": "RGaCBFg9CsDYVvm2kchbcw", 
	        "time": 1371960912
	    }, 
	    "data": {
	        "mood": {
	            "time_updated": 1370731786, 
	            "xid": "EJpCkyAtwoMpB8b_a5GOLQ", 
	            "title": "\u5f88\u5174\u594b\uff0c\u7761\u5f97\u4e0d\u9519", 
	            "time_created": 1370731760, 
	            "app_generated": false, 
	            "details": {
	                "tz": "Asia/Shanghai"
	            }, 
	            "date": 20130609, 
	            "shared": true, 
	            "type": "mood", 
	            "sub_type": 2
	        }, 
	        "move": {
	            "distance": 7.965, 
	            "longest_idle": 7260, 
	            "calories": 496.492103646, 
	            "bmr_calories_day": 1198.44050706, 
	            "goals": {
	                "steps": [
	                    11611.0, 
	                    10000
	                ], 
	                "workout_time": [
	                    1800.0, 
	                    null
	                ]
	            }, 
	            "longest_active": 1800, 
	            "hidden": false, 
	            "bg_steps": 11611.0, 
	            "bmr_calories": 1112.31734078, 
	            "active_time": 7917.0
	        }, 
	        "sleep": {
	            "awakenings": 2, 
	            "light": 12406.0, 
	            "time_to_sleep": 262, 
	            "goals": {
	                "total": [
	                    28542.0, 
	                    28800
	                ], 
	                "bedtime": [
	                    1317, 
	                    null
	                ], 
	                "deep": [
	                    16136.0, 
	                    null
	                ]
	            }, 
	            "qualities": [
	                99
	            ], 
	            "awake": 2189.0, 
	            "hidden": false
	        }, 
	        "user_metrics": {
	            "dob": 19760609, 
	            "gender": 1, 
	            "pal": null, 
	            "weight": 57.0, 
	            "height": 1.58
	        }, 
	        "meals": {
	            "num_meals": 1, 
	            "calories": 232.0, 
	            "num_drinks": 0, 
	            "goals": {
	                "carbs": [
	                    4.76000010967, 
	                    null
	                ], 
	                "fiber": [
	                    0.300000011921, 
	                    null
	                ], 
	                "sodium": [
	                    663.0, 
	                    null
	                ], 
	                "sugar": [
	                    0.939999982715, 
	                    null
	                ], 
	                "calcium": [
	                    70.0, 
	                    null
	                ], 
	                "unsat_fat": [
	                    9.69599956274, 
	                    null
	                ], 
	                "cholesterol": [
	                    460.0, 
	                    null
	                ], 
	                "protein": [
	                    15.3100000619, 
	                    null
	                ], 
	                "sat_fat": [
	                    4.5640001595, 
	                    null
	                ]
	            }, 
	            "hidden": false, 
	            "num_foods": 2
	        }, 
	        "insights": {
	            "items": []
	        }
	    }
	}

### users/%userXid%/social ###

**users/%userXid%/social** 用于查看用户各种活动的综合情况，按照当日或设定的截止访问时间，由近至远排列。

**Request：**

	GET
	https://jawbone.com/nudge/api/users/@me/social	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/social
	https://jawbone.com/nudge/api/users/@me/social?date=20130609	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/social?date=20130609
	https://jawbone.com/nudge/api/users/@me/social?date=20130609&limit=20	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/social?date=20130609&limit=20

**Params：**
	
    'date' : datestr #格式为yyyymmdd，如果没有 date 参数，返回今日数据
	'limit' : limit	 #整数，缺省为20。用于限制返回多少条结果

**Return :** 

返回信息包括 Mood、Move、Sleep、Meals、WorkOut 等活动的分项信息，此处数据仅为对应行为的概述性数据，如果需要更详细的分项数据汇总，需要以 "data"."feed"."xid" 为参数（记为 %evtXid% ），根据 "data"."feed"."type" 调用对应的接口。如：

	sleeps/%evtXid%
	workouts/%evtXid%
	moves/%evtXid%
	meals/%evtXid%
	mood/%evtXid%

	sleeps/%evtXid%/snapshot
	workouts/%evtXid%/snapshot
	moves/%evtXid%/snapshot

如果需要换页，可以在"data"."links"."next" 找到换页的URL，如：
`"data"."links"."next": "/nudge/api/v.1.34/users/RGaCBFg9CsDYVvm2kchbcw/social?page_token=1370439133&limit=20"` 

![Score](imgs/jawbone_up_3.png)

完整JSON示例如下：

	{
	    "meta": {
	        "code": 200, 
	        "message": "OK", 
	        "user_xid": "RGaCBFg9CsDYVvm2kchbcw", 
	        "time": 1371960914
	    }, 
	    "data": {
	        "feed": [
	            {
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370812786, 
	                "subtitle": null, 
	                "title": "6 \u5c0f\u65f6 40 \u5206\u949f", 
	                "quality": 78, 
	                "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoNhHfcKmXhLDQ/image/11370812792", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-Ypd7aQb52I3A", 
	                "app_generated": false, 
	                "awake": 806, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370812786, 
	                "duration": 24852, 
	                "xid": "EJpCkyAtwoNhHfcKmXhLDQ", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1370787960, 
	                "xid": "EJpCkyAtwoPJKCOzFyu89A", 
	                "title": "11,611 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/EJpCkyAtwoPJKCOzFyu89A/image/11370812791", 
	                "reached_goal": true, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-bWv8AvxdlIPg", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370730601, 
	                "date": 20130609, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "reaction": null, 
	                "time_updated": 1370732127, 
	                "tz": "Asia/Shanghai", 
	                "subtitle": null, 
	                "title": "Fried Egg \u548c Noodle Soup", 
	                "image": null, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "note": "Fried Egg \u548c Noodle Soup", 
	                "activity_xid": "lX5iJd5Y8-ZDW7VvQnqXGA", 
	                "app_generated": false, 
	                "details": {
	                    "carbohydrate": 4.76000010967, 
	                    "saturated_fat": 4.5640001595, 
	                    "protein": 15.3100000619, 
	                    "tz": "Asia/Shanghai", 
	                    "sodium": 663, 
	                    "vitamin_c": 0, 
	                    "vitamin_a": 0, 
	                    "unsaturated_fat": 9.69599956274, 
	                    "sugar": 0.939999982715, 
	                    "num_drinks": 0, 
	                    "accuracy": 0.0, 
	                    "fiber": 0.300000011921, 
	                    "potassium": 0, 
	                    "fat": 0, 
	                    "num_foods": 2, 
	                    "monounsaturated_fat": 0, 
	                    "calories": 232, 
	                    "place_type": "", 
	                    "polyunsaturated_fat": 0, 
	                    "calcium": 70, 
	                    "iron": 0, 
	                    "cholesterol": 460
	                }, 
	                "time_created": 1370732127, 
	                "xid": "EJpCkyAtwoMKaoaitjJHmg", 
	                "place_name": "", 
	                "type": "meal", 
	                "networks": [], 
	                "is_private": true, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "time_updated": 1370731760, 
	                "tz": "Asia/Shanghai", 
	                "title": "\u5f88\u5174\u594b\uff0c\u7761\u5f97\u4e0d\u9519", 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-ZoG48PRRZa5g", 
	                "app_generated": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370731760, 
	                "xid": "EJpCkyAtwoMpB8b_a5GOLQ", 
	                "type": "mood", 
	                "sub_type": 2, 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "reaction": null, 
	                "time_updated": 1370730615, 
	                "subtitle": null, 
	                "title": "\u745c\u4f3d", 
	                "type": "workout", 
	                "is_completed": 1, 
	                "networks": [], 
	                "km": 0.0, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-Yth6ROIP2QPA", 
	                "app_generated": false, 
	                "steps": 0, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_created": 1370730615, 
	                "xid": "EJpCkyAtwoPa1a-leTNHQg", 
	                "duration": 1800, 
	                "tz": "Asia/Shanghai", 
	                "image": "/ver/static/images/up/Workout_Feedv2_yoga.png", 
	                "sub_type": 6, 
	                "is_private": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370730588, 
	                "subtitle": null, 
	                "title": "7 \u5c0f\u65f6 55 \u5206\u949f", 
	                "quality": 99, 
	                "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoPx-NDQaWEnSw/image/11370730602", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-aWWUdEvsk67g", 
	                "app_generated": false, 
	                "awake": 2189, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370730588, 
	                "duration": 30731, 
	                "xid": "EJpCkyAtwoPx-NDQaWEnSw", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1370699820, 
	                "xid": "EJpCkyAtwoPW1YD0X277hA", 
	                "title": "6,693 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/EJpCkyAtwoPW1YD0X277hA/image/11370730599", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-aUJI6pRVGHRQ", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370645647, 
	                "date": 20130608, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370644226, 
	                "subtitle": null, 
	                "title": "7 \u5c0f\u65f6 0 \u5206\u949f", 
	                "quality": 0, 
	                "image": "/ver/static/images/up/Sleep_Feed_Manual.png", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-bV0p1Rp8I6xw", 
	                "app_generated": false, 
	                "awake": 0, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370644226, 
	                "duration": 25200, 
	                "xid": "EJpCkyAtwoP5F4HBk72dng", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1370620620, 
	                "xid": "BXM3Lg0tIY39TAWZF9J3LA", 
	                "title": "13,030 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/BXM3Lg0tIY39TAWZF9J3LA/image/11370645645", 
	                "reached_goal": true, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-bAiFLwbwZ9IQ", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370575326, 
	                "date": 20130607, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370619840, 
	                "subtitle": null, 
	                "title": "1 \u5c0f\u65f6 29 \u5206\u949f", 
	                "quality": 18, 
	                "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoOF3sVPoMzjNw/image/11370645648", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-ap8twiX42Qtw", 
	                "app_generated": false, 
	                "awake": 1052, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370619840, 
	                "duration": 6394, 
	                "xid": "EJpCkyAtwoOF3sVPoMzjNw", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370557817, 
	                "subtitle": null, 
	                "title": "5 \u5c0f\u65f6 56 \u5206\u949f", 
	                "quality": 64, 
	                "image": "/nudge/api/v.1.34/sleeps/BXM3Lg0tIY0DwwLlUXmstA/image/11370557855", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-Yb3mDOv7V1qA", 
	                "app_generated": false, 
	                "awake": 3297, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370557817, 
	                "duration": 24682, 
	                "xid": "BXM3Lg0tIY0DwwLlUXmstA", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1370533080, 
	                "xid": "BXM3Lg0tIY2dRqV_zVk94A", 
	                "title": "12,339 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/BXM3Lg0tIY2dRqV_zVk94A/image/11370557868", 
	                "reached_goal": true, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-bV19tQDrHFBg", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370472153, 
	                "date": 20130606, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1370471100, 
	                "subtitle": null, 
	                "title": "5 \u5c0f\u65f6 35 \u5206\u949f", 
	                "quality": 71, 
	                "image": "/nudge/api/v.1.34/sleeps/BXM3Lg0tIY2H-2uHcDwYSg/image/11370472155", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-b9nP8F8UDNHA", 
	                "app_generated": false, 
	                "awake": 2082, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370471100, 
	                "duration": 22233, 
	                "xid": "BXM3Lg0tIY2H-2uHcDwYSg", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1370447940, 
	                "xid": "BXM3Lg0tIY14vuEzUfC9QA", 
	                "title": "367 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/BXM3Lg0tIY14vuEzUfC9QA/image/11370447939", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-aHK_ym9GCZIA", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370439710, 
	                "date": 20130605, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "time_updated": 1370439133, 
	                "tz": "Asia/Shanghai", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "lX5iJd5Y8-aRU1FwEZIFfQ", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370439133, 
	                "type": "user_joined", 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }
	        ], 
	        "links": {
	            "next": "/nudge/api/v.1.34/users/RGaCBFg9CsDYVvm2kchbcw/social?page_token=1370439133&limit=20"
	        }
	    }
	}

## Users Feeds ##

### users/%userXid%/feed ###

users/%userXid%/feed 这个 API 和 users/%userXid%/social 几乎如出一辙，唯一差别就是 social API多了个参数 date, 两者的返回参数几乎完全相同

**Request：**

	GET
	https://jawbone.com/nudge/api/users/@me/feed	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/feed
	https://jawbone.com/nudge/api/users/@me/feed?limit=5	#返回结果与下条完全一致
	https://jawbone.com/nudge/api/users/RGaCBFg9CsDYVvm2kchbcw/feed?limit=5

**Params：**
	
	'limit' : limit	 #整数，缺省为10。用于限制返回多少条结果

**Return :** 

如果需要换页，可以在"data"."links"."next" 找到换页的URL，如：`"data"."links"."next": "/nudge/api/v.1.34/users/RGaCBFg9CsDYVvm2kchbcw/feed?page_token=1371815940&limit=5"` 

完整JSON示例如下：

	{
	    "meta": {
	        "code": 200, 
	        "message": "OK", 
	        "user_xid": "RGaCBFg9CsDYVvm2kchbcw", 
	        "time": 1371961008
	    }, 
	    "data": {
	        "feed": [
	            {
	                "time_updated": 1371959462, 
	                "xid": "EJpCkyAtwoO25G_p4fNiyw", 
	                "title": "\u4eca\u65e5 940 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/EJpCkyAtwoO25G_p4fNiyw/image/11371959462", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "Qt_j0hDUsXnI65eWs053WA", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1371959462, 
	                "date": 20130623, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1371949680, 
	                "subtitle": null, 
	                "title": "6 \u5c0f\u65f6 54 \u5206\u949f", 
	                "quality": 78, 
	                "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoPLz1cyhrWHeA/image/11371959463", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "Qt_j0hDUsXmXwgN2HMMi9w", 
	                "app_generated": false, 
	                "awake": 2217, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1371949680, 
	                "duration": 27108, 
	                "xid": "EJpCkyAtwoPLz1cyhrWHeA", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1371915360, 
	                "xid": "EJpCkyAtwoPqi0m-9ZxLSQ", 
	                "title": "6,412 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/EJpCkyAtwoPqi0m-9ZxLSQ/image/11371959461", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "Qt_j0hDUsXl4mei5FOz3vw", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1371860650, 
	                "date": 20130622, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "user": {
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "last": "VisHealth", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "time_updated": 1371854400, 
	                "subtitle": null, 
	                "title": "8 \u5c0f\u65f6 12 \u5206\u949f", 
	                "quality": 100, 
	                "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoMo-EN05aYhog/image/11371860652", 
	                "reached_goal": true, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "Qt_j0hDUsXlrqp2cjqcfuw", 
	                "app_generated": false, 
	                "awake": 519, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1371854400, 
	                "duration": 30039, 
	                "xid": "EJpCkyAtwoMo-EN05aYhog", 
	                "type": "sleep", 
	                "networks": [], 
	                "is_private": true, 
	                "tz": "Asia/Shanghai"
	            }, 
	            {
	                "time_updated": 1371815940, 
	                "xid": "EJpCkyAtwoM9d9ABSHCTRw", 
	                "title": "4,616 \u6b65", 
	                "image": "/nudge/api/v.1.34/moves/EJpCkyAtwoM9d9ABSHCTRw/image/11371860647", 
	                "reached_goal": false, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "activity_xid": "Qt_j0hDUsXk8mod8pvWZUA", 
	                "app_generated": false, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1371767150, 
	                "date": 20130621, 
	                "tz": "Asia/Shanghai", 
	                "type": "move", 
	                "networks": [], 
	                "is_private": false, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }
	        ], 
	        "links": {
	            "next": "/nudge/api/v.1.34/users/RGaCBFg9CsDYVvm2kchbcw/feed?page_token=1371815940&limit=5"
	        }
	    }
	}

### feeditems/%activityXid% ###

在 users/%userXid%/social 和 users/%userXid%/feed 两个 API 中的返回值中，我们会发现每个 "data"."feed" 都有一个叫做 "activity_xid" 的值，这个值能够作为 feeditems/%activityXid% 的调用参数，返回单个行为的概述数据。我比对了一下，发现其返回 JSON 数据和上两个 API 的返回值中的数据似乎没什么区别。

**Request：**

	GET
	https://jawbone.com/nudge/api/nudge/api/feeditems/Qt_j0hDUsXmXwgN2HMMi9w

**Params：**
	
	无，URL 为 REST 形式，将 activityXid 放在了 URL 中

**Return :** 

完整JSON示例如下：

	{
	    "meta": {
	        "code": 200, 
	        "message": "OK", 
	        "user_xid": "RGaCBFg9CsDYVvm2kchbcw", 
	        "time": 1371961011
	    }, 
	    "data": {
	        "user": {
	            "last": "VisHealth", 
	            "name": "Tester VisHealth", 
	            "short_name": "Tester", 
	            "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	            "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	            "type": "user", 
	            "first": "Tester"
	        }, 
	        "time_updated": 1371949680, 
	        "subtitle": null, 
	        "title": "6 \u5c0f\u65f6 54 \u5206\u949f", 
	        "quality": 78, 
	        "image": "/nudge/api/v.1.34/sleeps/EJpCkyAtwoPLz1cyhrWHeA/image/11371959463", 
	        "reached_goal": false, 
	        "comments": {
	            "items": [], 
	            "size": 0
	        }, 
	        "activity_xid": "Qt_j0hDUsXmXwgN2HMMi9w", 
	        "app_generated": false, 
	        "awake": 2217, 
	        "emotions": {
	            "items": [], 
	            "size": 0
	        }, 
	        "time_created": 1371949680, 
	        "duration": 27108, 
	        "xid": "EJpCkyAtwoPLz1cyhrWHeA", 
	        "type": "sleep", 
	        "networks": [], 
	        "is_private": true, 
	        "tz": "Asia/Shanghai"
	    }
	}

## Users Events ##

### users/%userXid%/events ###

users/%userXid%/events 是个非常有用的函数。 利用这个函数，能够获取当前用户的所有 Event， 包括已经被删除的用户事件。同时，这个 API 还有 types 参数，能够限制只返回特定类别 Event。

**Request：**

	GET
	https://jawbone.com/nudge/api/users/@me/events?list_deleted=True&limit=10
	https://jawbone.com/nudge/api/users/@me/events?limit=20&start_date=20130609&types=2
	https://jawbone.com/nudge/api/users/@me/events?types=1%2C3 # 此处逗号被转码为%2C了

**Params：**
	
	'start_date' : startDate, # 开始日期，格式为yyyymmdd
	'types' : types,  # 选择哪种类型的事件。取值为 : 1 workout, 2 meal, 3 sleep, 4 move, 5 mood, 7 body。需要显示多个类型，可以将类型取值用逗号连接。不设置则包括所有类型。此处取值为逆向工程得出，不一定全面。
	'limit' : limit ,  # 最多返回多少条记录，缺省为20。
	"list_deleted" : listDeleted # 取值为 True， False。缺省为 False。

**Return :** 

完整JSON示例如下：

	{
	    "meta": {
	        "code": 200, 
	        "message": "OK", 
	        "user_xid": "RGaCBFg9CsDYVvm2kchbcw", 
	        "time": 1372431968
	    }, 
	    "data": {
	        "deleted": [
	            "EJpCkyAtwoOzTldhXELa5Q", 
	            "EJpCkyAtwoOMMWEHyIFF0Q"
	        ], 
	        "items": [
	            {
	                "image": "", 
	                "time_removed": 0, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "snapshot_image": "/nudge/image/e/1371044854/EJpCkyAtwoPXN30hjKufWg.png", 
	                "networks": [], 
	                "time_completed": 1371016848, 
	                "xid": "EJpCkyAtwoPXN30hjKufWg", 
	                "title": "\u6b65\u884c", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "details": {
	                    "tz": "Asia/Shanghai", 
	                    "goal": 0, 
	                    "calories": 802.075100205, 
	                    "km": 2.249, 
	                    "bmr": 164.075100205, 
	                    "intensity": 1, 
	                    "bg_calories": 127.953000784, 
	                    "meters": 2249, 
	                    "time": 10800, 
	                    "bg_active_time": 1871, 
	                    "steps": 3372, 
	                    "bmr_calories": 164.075100205
	                }, 
	                "shared": true, 
	                "type": "workout", 
	                "band_ids": [], 
	                "time_created": 1371006048, 
	                "date": 20130612, 
	                "sub_type": 1, 
	                "reaction": null, 
	                "time_updated": 1371044854, 
	                "route": "", 
	                "app_generated": false, 
	                "goals": {
	                    "steps": 10000, 
	                    "workout_time": null
	                }, 
	                "is_manual": true, 
	                "is_complete": true, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "time_completed": 1370995140, 
	                "xid": "EJpCkyAtwoOBmMCiBEugow", 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "9 \u5c0f\u65f6 55 \u5206\u949f", 
	                "snapshot_image": "/nudge/image/e/1370995674/EJpCkyAtwoOBmMCiBEugow.png", 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "sub_type": 0, 
	                "date": 20130612, 
	                "app_generated": false, 
	                "time_updated": 1370995674, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370958101, 
	                "is_manual": false, 
	                "shared": false, 
	                "type": "sleep", 
	                "networks": [], 
	                "goals": {
	                    "total": 27000, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 1370988000, 
	                    "awakenings": 1, 
	                    "light": 20652, 
	                    "mind": 0, 
	                    "asleep_time": 1370958959, 
	                    "deep": 15093, 
	                    "awake": 1294, 
	                    "duration": 37039, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 100, 
	                    "awake_time": 1370995001
	                }
	            }, 
	            {
	                "time_updated": 1370957155, 
	                "xid": "EJpCkyAtwoMIuTGkAPvdTw", 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "1 \u5c0f\u65f6 7 \u5206\u949f", 
	                "snapshot_image": "/nudge/image/e/1370957155/EJpCkyAtwoMIuTGkAPvdTw.png", 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "date": 20130611, 
	                "app_generated": false, 
	                "time_completed": 1370908500, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370902821, 
	                "is_manual": false, 
	                "shared": false, 
	                "type": "sleep", 
	                "sub_type": 2, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 0, 
	                    "light": 1680, 
	                    "mind": 0, 
	                    "asleep_time": 1370903099, 
	                    "deep": 2340, 
	                    "awake": 1659, 
	                    "duration": 5679, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 12, 
	                    "awake_time": 1370907021
	                }
	            }, 
	            {
	                "time_updated": 1370957154, 
	                "xid": "EJpCkyAtwoPgu7Y5xcoEUw", 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "4 \u5c0f\u65f6 23 \u5206\u949f", 
	                "type": "sleep", 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "is_manual": false, 
	                "app_generated": false, 
	                "time_completed": 1370898180, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370880162, 
	                "date": 20130611, 
	                "shared": false, 
	                "snapshot_image": "/nudge/image/e/1370957154/EJpCkyAtwoPgu7Y5xcoEUw.png", 
	                "sub_type": 0, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 2, 
	                    "light": 11393, 
	                    "mind": 0, 
	                    "asleep_time": 1370881499, 
	                    "deep": 4408, 
	                    "awake": 2217, 
	                    "duration": 18018, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 46, 
	                    "awake_time": 1370898162
	                }
	            }, 
	            {
	                "time_updated": 1370812792, 
	                "xid": "EJpCkyAtwoNhHfcKmXhLDQ", 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 0, 
	                    "light": 13966, 
	                    "mind": 0, 
	                    "asleep_time": 1370788739, 
	                    "deep": 10080, 
	                    "awake": 806, 
	                    "duration": 24852, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 78, 
	                    "awake_time": 1370812534
	                }, 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "6 \u5c0f\u65f6 40 \u5206\u949f", 
	                "snapshot_image": "/nudge/image/e/1370812792/EJpCkyAtwoNhHfcKmXhLDQ.png", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "is_manual": false, 
	                "app_generated": false, 
	                "time_completed": 1370812786, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370787934, 
	                "date": 20130610, 
	                "shared": false, 
	                "type": "sleep", 
	                "sub_type": 0, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "image": "", 
	                "time_removed": 0, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "snapshot_image": "", 
	                "networks": [], 
	                "time_completed": 1370730615, 
	                "xid": "EJpCkyAtwoPa1a-leTNHQg", 
	                "title": "\u745c\u4f3d", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "details": {
	                    "tz": "Asia/Shanghai", 
	                    "goal": 0, 
	                    "calories": 89.4861169007, 
	                    "km": 0.0, 
	                    "bmr": 27.4861169007, 
	                    "intensity": 1, 
	                    "bg_calories": 0, 
	                    "meters": 0, 
	                    "time": 1800, 
	                    "bg_active_time": 0, 
	                    "steps": 0, 
	                    "bmr_calories": 27.4861169007
	                }, 
	                "shared": true, 
	                "type": "workout", 
	                "band_ids": [], 
	                "time_created": 1370728815, 
	                "date": 20130609, 
	                "sub_type": 6, 
	                "reaction": null, 
	                "time_updated": 1370732478, 
	                "route": "", 
	                "app_generated": false, 
	                "goals": {
	                    "steps": 10000, 
	                    "workout_time": null
	                }, 
	                "is_manual": true, 
	                "is_complete": true, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "time_completed": 1370730588, 
	                "xid": "EJpCkyAtwoPx-NDQaWEnSw", 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "7 \u5c0f\u65f6 55 \u5206\u949f", 
	                "type": "sleep", 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "sub_type": 0, 
	                "date": 20130609, 
	                "app_generated": false, 
	                "time_updated": 1370730602, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370699857, 
	                "is_manual": false, 
	                "shared": false, 
	                "snapshot_image": "/nudge/image/e/1370730602/EJpCkyAtwoPx-NDQaWEnSw.png", 
	                "networks": [], 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 2, 
	                    "light": 12406, 
	                    "mind": 0, 
	                    "asleep_time": 1370700119, 
	                    "deep": 16136, 
	                    "awake": 2189, 
	                    "duration": 30731, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 99, 
	                    "awake_time": 1370729557
	                }
	            }, 
	            {
	                "time_completed": 1370644226, 
	                "xid": "EJpCkyAtwoP5F4HBk72dng", 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 0, 
	                    "light": 0, 
	                    "mind": 0, 
	                    "asleep_time": 1370619026, 
	                    "deep": 0, 
	                    "awake": 0, 
	                    "duration": 25200, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 0, 
	                    "awake_time": 1370644226
	                }, 
	                "band_ids": [], 
	                "title": "7 \u5c0f\u65f6 0 \u5206\u949f", 
	                "snapshot_image": "", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "date": 20130608, 
	                "app_generated": false, 
	                "time_updated": 1370732654, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370619026, 
	                "is_manual": true, 
	                "shared": false, 
	                "type": "sleep", 
	                "sub_type": 0, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }, 
	            {
	                "time_completed": 1370619840, 
	                "xid": "EJpCkyAtwoOF3sVPoMzjNw", 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "1 \u5c0f\u65f6 29 \u5206\u949f", 
	                "type": "sleep", 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }, 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "date": 20130607, 
	                "app_generated": false, 
	                "time_updated": 1370645648, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370613446, 
	                "is_manual": false, 
	                "shared": false, 
	                "snapshot_image": "/nudge/image/e/1370645648/EJpCkyAtwoOF3sVPoMzjNw.png", 
	                "sub_type": 2, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 0, 
	                    "awakenings": 0, 
	                    "light": 2762, 
	                    "mind": 0, 
	                    "asleep_time": 1370614439, 
	                    "deep": 2580, 
	                    "awake": 1052, 
	                    "duration": 6394, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 18, 
	                    "awake_time": 1370619746
	                }
	            }, 
	            {
	                "time_completed": 1370557817, 
	                "xid": "BXM3Lg0tIY0DwwLlUXmstA", 
	                "details": {
	                    "body": 0, 
	                    "smart_alarm_fire": 1370557200, 
	                    "awakenings": 2, 
	                    "light": 14160, 
	                    "mind": 0, 
	                    "asleep_time": 1370533799, 
	                    "deep": 7225, 
	                    "awake": 3297, 
	                    "duration": 24682, 
	                    "tz": "Asia/Shanghai", 
	                    "quality": 64, 
	                    "awake_time": 1370557135
	                }, 
	                "band_ids": [
	                    "23424880A48CD061"
	                ], 
	                "title": "5 \u5c0f\u65f6 56 \u5206\u949f", 
	                "snapshot_image": "/nudge/image/e/1370557855/BXM3Lg0tIY0DwwLlUXmstA.png", 
	                "comments": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "networks": [], 
	                "date": 20130607, 
	                "app_generated": false, 
	                "time_updated": 1370557855, 
	                "emotions": {
	                    "items": [], 
	                    "size": 0
	                }, 
	                "time_created": 1370533135, 
	                "is_manual": false, 
	                "shared": false, 
	                "type": "sleep", 
	                "sub_type": 0, 
	                "goals": {
	                    "total": 28800, 
	                    "bedtime": null, 
	                    "deep": null
	                }, 
	                "user": {
	                    "last": "VisHealth", 
	                    "name": "Tester VisHealth", 
	                    "short_name": "Tester", 
	                    "image": "user/image/i/51b4916b03eb185d1c1948a5_RGaCBFg9CsDYVvm2kchbcw_137078820345_2781804_photo.jpeg", 
	                    "xid": "RGaCBFg9CsDYVvm2kchbcw", 
	                    "type": "user", 
	                    "first": "Tester"
	                }
	            }
	        ], 
	        "earliest": 20130605, 
	        "size": 10
	    }
	}

---
[返回](how-to-fetch-jawbone-data-unofficial-api.md)