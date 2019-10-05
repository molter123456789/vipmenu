#include <sourcemod>
#include <sdktools>

//Compilování
#pragma semicolon 1
#pragma newdecls required

/*
    Plugin start
*/

public Plugin myinfo = {
    name        = "[CS:GO] VIP Menu",
    author      = "molter",
    description = "VIP Plugin - placená konfigurace",
    version     = "1.0",
    url         = "https://github.com/molter123456789"
};

public void OnPluginStart()
{
    //Commands
    RegConsoleCmd("sm_vipmenu", Command_VIPMenu);
}

/*
    Commands
*/

public Action Command_VIPMenu(int client, int args)
{
    if(IsValidClient(client))
    {
        if(IsClientVIP(client))
        {
            Menu_VIPMenu(client);
        }
        else
        {
            ReplyToCommand(client, "Tento příkaz je pouze pro VIP hráče!");
        }
    }

    return Plugin_Handled;
}

/*
    Menus
*/

void Menu_VIPMenu(int client)
{
    Menu menu = new Menu(mVIPMenu);

    menu.SetTitle("VIP Menu:");

    menu.AddItem("adr", "Adrenalín");
    menu.AddItem("ak", "Vybrat AK-47 + Deagle");
    menu.AddItem("m4", "Vybrat M4A4 + Deagle");
    menu.AddItem("awp", "Vybrat AWP + Deagle");
    menu.AddItem("gran", "Gránáty + armor");

    menu.Display(client, MENU_TIME_FOREVER);
}

public int mVIPMenu(Menu menu, MenuAction action, int client, int index)
{
    switch(action)
    {
        case MenuAction_Select:
        {
            if(IsValidClient(client))
            {
                if(IsClientVIP(client))
                {
                    char szItem[32];
                    menu.GetItem(index, szItem, sizeof(szItem));

                    if(StrEqual(szItem, "adr"))
                    {
                        GivePlayerItem(client, "weapon_healthshot");
                    }
                    else if(StrEqual(szItem, "ak"))
                    {
                        GivePlayerItem(client, "weapon_ak47");
                        GivePlayerItem(client, "weapon_deagle");
                    }
                    else if(StrEqual(szItem, "m4"))
                    {
                        GivePlayerItem(client, "weapon_m4a1");
                        GivePlayerItem(client, "weapon_deagle");
                    }
                    else if(StrEqual(szItem, "awp"))
                    {
                        GivePlayerItem(client, "weapon_awp");
                        GivePlayerItem(client, "weapon_deagle");
                    }
                    else if(StrEqual(szItem, "gran"))
                    {
                        GivePlayerItem(client, "weapon_flashbang");
                        GivePlayerItem(client, "weapon_hegrenade");
                        GivePlayerItem(client, "weapon_smokegrenade");
                        GivePlayerItem(client, "item_assaultsuit");
                    }
                }
            }
        }
    }
}

/*
    Stocks
*/

stock bool IsValidClient(int client, bool alive = false)
{
    if(client >= 1 && client <= MaxClients && IsClientConnected(client) && IsClientInGame(client) && !IsClientSourceTV(client) && (alive == false || IsPlayerAlive(client)))
    {
        return true;
    }
    
    return false;
}

stock bool IsClientVIP(int client)
{
    return CheckCommandAccess(client, "", ADMFLAG_RESERVATION);
}
