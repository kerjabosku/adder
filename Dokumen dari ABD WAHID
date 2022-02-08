import json,os,datetime,time
from telethon.sync import TelegramClient
from telethon.tl.functions.messages import GetDialogsRequest
from telethon.tl.functions.channels import GetParticipantsRequest, GetFullChannelRequest, InviteToChannelRequest
from telethon.tl.types import InputPeerEmpty, ChannelParticipantsSearch,UserStatusOffline, UserStatusRecently, InputPeerChannel, InputPeerUser
from telethon.errors.rpcerrorlist import PeerFloodError, UserPrivacyRestrictedError

lgr = "\033[90m"
lr = "\033[91m"
lg = "\033[92m"
ly = "\033[93m"
lb = "\033[94m"
lc = "\033[96m"
lx = "\033[0m"

def banner(user=None):
	user = user.get_me().first_name if user else "User"
	banner_text = f"""{lc}

▀█▀ █▀▀ █░░ █▀▀ ▀█▀ █▀█ █▀█ █░░
░█░ ██▄ █▄▄ ██▄ ░█░ █▄█ █▄█ █▄▄ {ly}V0.2
{lgr}By https://t.me/om_karjok
{lg}Welcome {ly}{user}{lg} !
{lx}"""
	os.system('clear')
	print(banner_text)
def loading(delay):
	rg = delay
	for i in range(rg):
		try:
			print(f"\r{lgr}    Please wait ({rg-(i+1)}s){lx}",end="",flush=True)
			time.sleep(1)
		except:
			exit()
	print()
def date_format(message):
	"""
	:param message:
	:return:
	"""
	if type(message) is datetime:
		return message.strftime("%Y-%m-%d %H:%M:%S")
def paginate(data,col=2):
	n_data = round(len(data)/col) + 1
	new_data_part = []
	batas = 0
	for i in range(n_data):
		new_data = []
		for x in range(batas,col+batas):
			try:
				new_data.append(data[x])
			except:
				pass
			batas += 1
		if new_data: new_data_part.append(new_data)
	return new_data_part

def init():
	banner()
	try:
		os.mkdir(".tmp")
	except:
		pass
	try:
		os.mkdir(".users")
	except:
		pass
	try:
		os.mkdir("results")
	except:
		pass

	if not "user.config" in os.listdir(".tmp"):
		json.dump({"phone":None},open(".tmp/user.config","w"))
	phone = json.load(open(".tmp/user.config"))['phone']
	if not phone:
		list_user = os.listdir(".users")
		if not list_user:
			phone = input(f"{lg}[*]{lx} Phone Number (ex:+62812..): ")
			while not "+62" in phone:
				print(f'{lr}[*]{lx} Please use +62 code in your number')
				phone = input(f"{lg}[*]{lx}Phone Number (ex:+62812..): ")
		else:
			print(f"{lg}[*]{lx} You have logged in account before.\nSelect User")
			for usr in list_user:
				print(f"   {lg}{n}.{lx} {usr}")
				n += 1
			us = input(f"{lg}[*]{lx} Select or Enter to login to another account: ")
			if us:
				cred = json.load(open(f".users/{list_user[int(us)-1]}"))
				phone = "+"+cred["phone"]
			else:
				phone = input(f"{lg}[*]{lx} Phone Number (ex:+62812..): ")
				while not "+62" in phone:
					print(f'{lr}[*]{lx} Please use +62 code in your number')
					phone = input(f"{lg}[*]{lx}Phone Number (ex:+62812..): ")
	client = login(phone)
	main_menu(client)
def main_menu(client):
	banner(client)
	print(f"""
    Select Menu
    
1. Add Member To Your Group
2. Get Group Member ID 
3. Account Menu
4. Reset Tool
5. Exit
""")
	menu = input(f"{lg}[*]{lx} Input Menu : ")
	if menu in ["1","01"]:
		menu_add_members(client)
	elif menu in ["2","02"]:
		get_group_members(client)
	elif menu in ["3","03"]:
		menu_account(client)
	elif menu in ["4","04"]:
		dlt = input("Reset tool ? [y/n]: ")
		if dlt == "y":
			try:
				os.system("rm -rf .tmp")
				os.system("rm -rf .users")
				os.system("rm -rf results")
				os.system("rm  *.session")
			except:
				pass
	else:
		exit()
def menu_account(client):
	user = client.get_me()
	banner(client)
	print("""
1. Current account detail
2. Add new account
3. Move to other account
4. Delete account

Enter to back
""")
	opt = input(f"{lg}[*]{lx} Select Menu : ")
	if opt == "1":
		uid = user.id if user.id else "-"
		ufname = user.first_name if user.first_name else "-"
		ulname = user.last_name if user.last_name else "-"
		uname = "@"+user.username if user.username else "-"
		uphone = "+"+user.phone if user.phone else "-"
		detail = f"""
Your Account Details
{lg}--------------------{lx}

ID           : {uid}
First Name   : {ufname}
Last Name    : {ulname}
Username     : {uname}
Phone Number : {uphone}
"""
		print(detail)
		input("Enter to back ")
		menu_account(client)

	elif opt == "2":
		phone = input(f"{lg}[*]{lx}Phone Number (ex:+62812..): ")
		client = login(phone)
		main_menu(client)
	elif opt == "3":
		n = 1
		list_user = os.listdir(".users")
		if len(list_user) != 0:
			print(f"{lg}[*]{lx} Select User")
			for usr in list_user:
				print(f"   {lg}{n}.{lx} {usr}")
				n += 1
			us = input(f"{lg}[*]{lx} Select: ")
			if us:
				cred = json.load(open(f".users/{list_user[int(us)-1]}"))
				phone = "+"+cred["phone"]
				if cred["phone"] == user.phone:
					print(f"{ly}[*]{lx} This is your current logged in account. Select another one or add new account")
					exit()
				else:
					client = login(phone)
					main_menu(client)
					
		else:
			print(f"{ly}[*]{lx} You dont have any account yet. Please login.")
			exit()
			
	elif opt == "4":
		n = 1
		list_user = os.listdir(".users")
		if len(list_user) != 0:
			print(f"{lg}[*]{lx} Select Account To Delete")
			for usr in list_user:
				print(f"   {lg}{n}.{lx} {usr}")
				n += 1
			us = input(f"{lg}[*]{lx} Select: ")
			if us:
				cred = json.load(open(f".users/{list_user[int(us)-1]}"))
				phone = "+"+cred["phone"]
				if cred["phone"] == user.phone:
					dlt = input(f"{ly}[*]{lx} This is your current logged in account. Delete ? [y/n] : ")
					if dlt != "y":
						exit()
				print(f"{lg}[*]{lx} Deleting {cred['first_name']} account..")
				sess = phone + ".session"
				os.remove(sess)
				os.remove(f".users/{list_user[int(us)-1]}")
				os.remove(f".tmp/user.config")
				print(f"{lg}[*]{lx} Done !")
				exit()

		else:
			print(f"{ly}[*]{lx} You dont have any account yet. Please login.")
			exit()
	else:
		main_menu(client)
		
		
def menu_add_members(client):
	banner(client)
	print(f"{lr}NOTE: {lx}This is an illegal tool. Using this tool means that you are fully responsible for what happens to you and your Telegram account.")
	print(f"{ly}TIPS: {lx}Use this tool properly. the maximum adding users to grub is 200 people per account. Every 50 users, the account will be limited to ±15 minutes to add the next user. If this happened to your account, please use another account.")
	#print(f"\n\n{lg}[*]{lx} Enter limit of add member (max 200/account) or pass to use default limit (50)")
	#limit = input(f"{lg}   > {lx}")
	print(f"{lg}[*]{lx} Enter delay time in seconds or pass to use default delay (60s)")
	delay = input(f"{lg}   > {lx}")
	if delay:
		print(f"{lg}[*]{lx} Use custom delay ({delay}s)")
		add_member(client,delay=int(delay))
	#elif limit and not delay:
	#	print(f"{lg}[*]{lx} Use custom limit ({limit} users) and default delay (60s)")
	#	add_member(client,limit=int(limit))
	#elif delay and not limit:
	#	print(f"{lg}[*]{lx} Use default limit (50 users) and custom delay ({delay}s)")
	#	add_member(client,delay=int(delay))
	else:
		print(f"{lg}[*]{lx} Use default delay (60s)")
		add_member(client)
		
	

def login(phone=None):
	print(f"{lg}[*]{lx} Login..")
	api_id = 7522207
	api_hash = 'b6c48d25d198071cb1a510cd1ce6dbc2'
	try:
		client = TelegramClient(phone, api_id, api_hash)
		client.connect()
		if not client.is_user_authorized():
			client.send_code_request(phone)
			client.sign_in(phone, input(f'{lg}[*]{lx} Enter the code: '))
		json.dump({"phone":phone},open(".tmp/user.config","w"))
		json.dump(client.get_me().to_dict(), open(f".users/{client.get_me().first_name}-{client.get_me().id}.json","w"), default=date_format)
		user = client.get_me()
		user_name = user.first_name
		return client
	except Exception as e:
		print(f"{lr}[*]{lx} Error occured when login: {ly}{e}{lx}")
		
def isActive(user):
	is_active = False
	status = user.status
	if isinstance(status, UserStatusOffline):
		return (datetime.datetime.now(tz=datetime.timezone.utc) - status.was_online) <= datetime.timedelta(days=1)
	elif isinstance(status,UserStatusRecently):
		return True
	else:
		return False
		
def get_group_list(client,add=False):
	chats = []
	last_date = None
	chunk_size = 200
	groups=[]
	result = client(GetDialogsRequest(
				offset_date=last_date,
				offset_id=0,
				offset_peer=InputPeerEmpty(),
				limit=chunk_size,
				hash = 0
				))
	chats.extend(result.chats)
	for chat in chats:
		dik = chat.to_dict()
		# check if chat type is group, not channel
		if "invite_users" in str(dik):
			#print(json.dumps(dik, default=date_format,indent=2))
			if add:
				if dik["admin_rights"]:
					groups.append(chat)
			else:
				if not dik["admin_rights"]:
					groups.append(chat)
	if len(groups) != 0:
		index = 1
		print(f"{lg}[*] {lx}Your group lists\n")
		for i in groups:
			print(f"    {lg}{index}.{lx} {i.title} ({ly}{i.participants_count} {lx}members)")
			index +=1
		sl = input(f"\n{lg}[*] {lx}Select Group: ")
		if not sl:
			main_menu(client)
		else:
			sltd = groups[int(sl) - 1]
			return sltd
	else:
		if add:
			print(f"{ly}[*]{lx} You don't have any group yet.\nPlease create one or make sure you have atleast 1 group that you are Admin")
		else:
			print(f"{ly}[*]{lx} You don't have join any group yet.\nPlease join to atleast 1 group and run again")
		exit()
			
def get_group_members(client):
	sltd = get_group_list(client)
	print(f"{lg}[*]{lx} Fetching {ly}{sltd.title}{lx} active member from {lc}{sltd.participants_count}{lx} member totals")
	members = []
	all_members = client.get_participants(sltd, aggressive=True)
	for member in all_members:
		u_id = member.id
		u_hash = member.access_hash
		f_name = member.first_name if member.first_name else ""
		l_name = member.last_name if member.last_name else ""
		u_name = member.username if member.username else ""
		u_phone = member.phone if member.phone else ""
		data = {
				"first_name":f_name,
				"last_name":l_name,
				"username":u_name,
				"id":u_id,
				"hash":u_hash,
				"phone":u_phone
				}
		if isActive(member):
			members.append(data)
			print(f"\r{lg}[*] {lx}Fetched active member: {ly}{round((len(members)/sltd.participants_count) * 100,2)}%{lx}..",end="",flush=True)
	print(f"\n{lg}[*] {ly}{round((len(members)/sltd.participants_count) * 100,2)}%{lx} ({lc}{len(members)}{lx}) active member(s) successfully fetched")
	sv = input(f"{lg}[*]{lx} Save as default file name (members-{sltd.title}.json) or custom ? [d/c]: ")
	if sv.upper() == "D":
		fname = f"members-{sltd.title}.json"
	else:
		fname = input(f"{lg}[*]{lx} Enter file name (ex: myid.json) : ")
	print(f"{lg}[*]{lx} Saving result as \"{ly}results/{fname}{lx}\"..")
	json.dump({"amounts":len(members),"data":members},open(f"results/{fname}","w"),indent=2)
	print(f"{lg}[*]{lx} Done!\nFile saved as {ly}{fname}{lx}")
	exit()

def add_member(client,limit=50,delay=60):
	print(f"{lg}[*]{lx} Select group for add new member")
	selected_group = get_group_list(client,add=True)
	target_group = InputPeerChannel(selected_group.id,selected_group.access_hash)
	if os.listdir("results"):
		print(f"{lg}[*]{lx} Select ID file")
		index = 1
		for i in os.listdir("results"):
			print(f"    {lg}{index}.{lx} {i}")
			index += 1
		sltd_id = json.load(open("results/"+os.listdir("results")[int(input(f"{lg}[*]{lx} Select: ")) -1]))
		print(f"{lg}[*]{lx} {ly}{len(sltd_id['data'])}{lx} ID loaded !")
		n = 1
		member_part = paginate(sltd_id["data"],50)
		"""
		indx = 1
		start = 1
		print(f"{lg}[*]{lx} Select index of ID from ID list")
		for data in member_part:
			number = f"{lg}{format(indx,str(len(str(len(member_part)))))}. {lx}"
			print(number,end="")
			indx += 1
			part = f"{format(start,str(len(str(len(sltd_id['data'])))))} {ly}-{lx} {start + len(data) - 1}"
			start += len(data)
			print(part)
		print()
		slx = input(f"{lg}[*]{lx} Select index: ")
		slxtd = member_part[int(slx)-1]
		"""
		print(f"{lg}[*]{lx} Input index (ex: 100-200)")
		idx = input(f"{lg}[*]{lx} Input: ")
		start,end = idx.split("-")
		slxtd = sltd_id["data"][int(start)-1:int(end)-1]
		print(f"{lg}[*]{lx} {len(slxtd)} IDs selected")
		print(f"{lg}[*]{lx} Starting !")
		for user in slxtd:
			try:
				user_to_add = InputPeerUser(user['id'], user['hash'])
				print(f'  {lg}{n}.{lx} Add {lc}{user["first_name"]} {user["last_name"]}{lx} to {lc}{selected_group.title}{lx}..')
				n += 1
				client(InviteToChannelRequest(target_group,[user_to_add]))
				loading(delay)
			except UserPrivacyRestrictedError:
				print(f"    {lr}!{lx} User privacy setting don't allowing this tool to add him to our group. Skip.")
				continue
			except Exception as e:
				print(f"    {lr}!{lx} Unknown error: {ly}{e}{lx}")
				continue
	else:
		print(f"{ly}[*]{lx} You don't have ID list.")
		get_group_members(client)
		add_member(client)
init()


