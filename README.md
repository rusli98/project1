# project1
chatbot telegram
import telebot
from telebot import types

bot = telebot.TeleBot("1708818096:AAFJ3kl-WPqhW04MOhmwrzjDkpZpfrxo_qg", parse_mode=None)
user_dict = {}

class User:
    def __init__(self, name):
        self.name = name
        self.q1= None

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
	chat_id = message.chat.id
	msg = bot.reply_to(message, "Assalamualaikum Kakak \n Saya Sistem Bot KURBAN BERKAH yang akan melayani kakak berbelanja hewan kurban.")
	bot.send_message(chat_id, 'Mari berkenalan, siapa nama kakak?')
	bot.register_next_step_handler(msg, process_name_step)

	# Logger
	print('nama : '+message.chat.first_name)
	

def process_name_step(message):
    try:
        chat_id = message.chat.id
        name = message.text
        user = User(name)
        user_dict[chat_id] = user
        bot.reply_to(message, 'Haloo '+ name)
        markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
        markup.add('Sapi', 'Kambing')
        msg = bot.reply_to(message, 'Hewan apa yang ingin kamu beli?? \n 1. Sapi \n 2. Kambing', reply_markup=markup)
        bot.register_next_step_handler(msg, process_pilih_step)
    except Exception as e:
        bot.reply_to(message, 'Kesalahan name step')


# proses pilih menu kambing atau sapi 
def process_pilih_step(message):
    try:
        chat_id = message.chat.id
        pilihan = message.text

        if pilihan == 'Sapi':
            markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
            markup.add('Sapi Lokal', 'Sapi Istimewa')
            msg = bot.reply_to(message, 'Sapi Jenis Apa yang ingin kamu Beli ?? \n 1. Sapi Lokal => Rp 14.390.000,- \n 2. Sapi Istimewa => Rp 16.500.000,-', reply_markup=markup)
            bot.register_next_step_handler(msg, process_pilihB_step)
        elif pilihan == 'Kambing':
            markup = types.ReplyKeyboardMarkup(one_time_keyboard=True)
            markup.add('Kambing FlashSale', 'Kambing Super')
            msg = bot.reply_to(message, 'Kambing Jenis Apa yang ingin kamu Beli ?? \n 1. Kambing FlashSale => Rp 1.590.000,- \n 2. Kambing Super => Rp 2.500.000,-', reply_markup=markup)
            bot.register_next_step_handler(msg, process_pilihA_step)
            
    except Exception as e:
        bot.reply_to(message, 'Kesalahan pilih step')

# proses pilih menu kambing
def process_pilihA_step(message):
    try:
        chat_id = message.chat.id
        pilihan = message.text
        user = user_dict[chat_id]
        bot.reply_to(message, 'Haloo '+ user.name + ' Pilihanmu adalah '+ pilihan + ' akan kami siapkan, silahkan isi data diri anda dan membayar sesuai tagihan')
        bot.reply_to(message, 'Nama : \nAlamat : \nNomor Telepon :  ')

        if pilihan == 'Kambing FlashSale':
            harga = 'Rp. 1.590.000,-'
        elif pilihan == 'Kambing Super':
            harga = 'Rp. 2.500.000,-'
        bot.send_message(chat_id, 'Silahkan kirim ke \nNo Rekening Mandiri kami :00993748323\nAN KURBAN BERKAH \nSebesar '+harga+' Terimakasih Telah Berbelanja di KURBAN BERKAH')
        bot.send_message(chat_id, 'Kirim Bukti Transaksi Pembayaran untuk proses validasi, Terimakasih')
        

    except Exception as e:
        bot.reply_to(message, 'Kesalahan result kambing step')


# proses pilih menu sapi
def process_pilihB_step(message):
    try:
        chat_id = message.chat.id
        pilihan = message.text
        user = user_dict[chat_id]
        bot.reply_to(message, 'Haloo '+ user.name + ' Pilihanmu adalah '+ pilihan + ' akan kami siapkan, silahkan isi data diri anda dan membayar sesuai tagihan')
        bot.reply_to(message, 'Nama : \nAlamat : \nNomor Telepon :  ')

        if pilihan == 'Sapi Lokal':
            harga = 'Rp. 14.390.000,-'
        elif pilihan == 'Sapi Istimewa':
            harga = 'Rp. 16.500.000,-'
        bot.send_message(chat_id, 'Silahkan kirim ke \nNo Rekening Mandiri kami :00993748323\nAN KURBAN BERKAH \nSebesar '+harga+' Terimakasih Telah Berbelanja di KURBAN BERKAH')
        bot.send_message(chat_id, 'Kirim Bukti Transaksi Pembayaran untuk proses validasi, Terimakasih')
        

    except Exception as e:
        bot.reply_to(message, 'Kesalahan result sapi step')




# proses pilih menu bayar
@bot.message_handler(content_types=['photo'])
def photo(message):
    chat_id = message.chat.id
    bot.reply_to(message, 'Telah Terkonfirmasi...Terima kasih Telah berbelanja di toko Kurban Berkah, paket akan segera disiapkan dan diantar sampai tujuan. Kami Tunggu pesanan selanjutnya, Salam KURBAN BERKAH...')
    



print('bot is running. . . ')
bot.polling()
