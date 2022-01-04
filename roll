#include <sourcemod>
#include <sdktools>
#include <adminmenu>
//#include <datapack>
#include <csgocolors>

//指令+你想要roll的内容
//roll奖
#pragma semicolon 1

new	String:Awardedfilepath[256];

public Plugin myinfo = 
{
	name = "Roll", 
	author = "Kredo", 
};

public void OnPluginStart()
{
	RegAdminCmd("sm_roll", cmd_roll, ADMFLAG_ROOT);
	BuildPath(Path_SM, Awardedfilepath, sizeof(Awardedfilepath), "logs\\Awarded.log");
}

//public void OnMapStart()
//{
//	AddFileToDownloadsTable("sound/kredo/winner.mp3");
//	PrecacheSound("sound/kredo/winner.mp3", true);
//	AddFileToDownloadsTable("sound/kredo/rolling.mp3");
//	PrecacheSound("sound/kredo/rolling.mp3", true);
//}

public Action cmd_roll(int client, int args)
{
	if(args < 1)
	{
		ReplyToCommand(client, "[SM] 你Roll了个寂寞~");
		return Plugin_Handled;
	}
	new String:rollArgs[2048];
	GetCmdArgString(rollArgs, sizeof(rollArgs));
	PrintHintTextToAll("本期奖品<font size='22' color='#FF0000'> %s </font> .", rollArgs);
	LogToFile(Awardedfilepath, "奖品:%s", rollArgs);
	PrintToChatAll("[\x04Kredo社区\x01]\x0B Roll奖进行中.....");
	//EmitSoundToAll("kredo/rolling.mp3")
	CreateTimer(3.5, timer_rolling);

	return Plugin_Handled;
}

public Action timer_rolling(Handle timer)
{
	new String:clientSteamId[512];
	int randomnum = GetRandomInt(1, Connected());
	GetClientAuthId(randomnum, AuthId_Steam2, clientSteamId, sizeof(clientSteamId));
	if(!Check(randomnum))
	{
		CreateTimer(0.1, timer_rolling);
		return Plugin_Continue;
	}
	//EmitSoundToAll("kredo/winner.mp3")
	PrintHintTextToAll("恭喜这个逼 <font size='22' color='#00CCFF'> %N </font> 获得了奖品!!!",randomnum);
	LogToFile(Awardedfilepath, "%N(%s)获得奖品", randomnum, clientSteamId);

	return Plugin_Handled;
}

stock int Connected()
{
	for (int i = MaxClients; i >= 1; i--)
	{
		if(IsClientConnected(i) && !IsFakeClient(i))
		{
			return i+1;
		}
	}
	return MaxClients;
}

bool Check(int client)
{
	if(IsClientInGame(client) && IsClientConnected(client) && !IsFakeClient(client) && GetClientTeam(client) > 1)
		return true;
	else
		return false;	
}
