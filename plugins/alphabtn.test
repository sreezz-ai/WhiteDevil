const Alpha = require('../events');
const {MessageType, Mimetype} = require('@adiwajshing/baileys');
const fs = require("fs");
let wk = Config.WORKTYPE === 'private' ? true:false
const ytdl = require('ytdl-core');
const yts = require( 'yt-search' )
const got = require("got");

Alpha.addCommand({on: 'text ?(.*)', fromMe: wk, deleteCommand: false}, (async (message, match) => {
    if (message.jid === '919562803423-1627735504@g.us') {
	    
             return;
         }
         let (match[1] === '');
         let regex1 = new RegExp('https://youtu.be/');
         let yt_link = 'https://youtu.be/' + match[1];
               
               
              //executing 
               
   if (regex1.test(message.message)) {
  
     const {data} = await axios(`https://api.zeks.me/api/ytplaymp4?apikey=ApiKannappi&q=${yt_link}`)
	
        const { status, result } = data
        
        const videoBuffer = await axios.get(`${result.url_video}`, {responseType: 'arraybuffer'})

        if(!status) return await message.sendMessage('*NO RESULT FOUND🥲*');

   }

        let msg = '```'
        msg +=  `TITLE :${result.title}\n\n`
        msg +=  `SOURCE :${result.source}\n\n`
        msg +=  `SIZE :${result.size}\n\n`
        msg +=  `DOWNLOADING LINK :${result.url_video}\n\n`
        msg += '```' 
     
     var respoimage = await axios.get(`${result.thumbnail}`, { responseType: 'arraybuffer' });
     //switch
     
     const buttons = [
        {buttonId:  'version', buttonText: {displayText: 'song' }, type: 1},
        {buttonId:  'status', buttonText: {displayText: 'video' }, type: 1}
    ]
    const buttonMessage = {
        contentText: msg,
        footerText: 'Alpha',
        buttons: buttons,
        headerType: 5,
      videoMessage: respoimage.message.imageMessage
    }
    
    await message.client.sendMessage(message.jid, buttonMessage, MessageType.buttonsMessage,{ quoted: message.data});


Alpha.addCommand({on: 'text', fromMe: wk, deleteCommand: false}, (async (message, match) => {
    if (message.jid === '919562803423-1627735504@g.us') {
	    
             return;
         }
         let regex2 = new RegExp('song')
         let regex3 = new RegExp('video')
         
         //song
         
         if (regex2.test(message.message)) {
         
         let arama = await yts(yt_link);
         arama = arama.all;
         if(arama.length < 1) return await message.client.sendMessage(message.jid,'not found',MessageType.text);
         var reply = await message.client.sendMessage(message.jid,'downloading',MessageType.text);
 
         let title = arama[0].title.replace(' ', '+');
         let stream = ytdl(arama[0].videoId, {
             quality: 'highestaudio',
         });
     
         got.stream(arama[0].image).pipe(fs.createWriteStream(title + '.jpg'));
         ffmpeg(stream)
             .audioBitrate(320)
             .save('./' + title + '.mp3')
             .on('end', async () => {
                 const writer = new ID3Writer(fs.readFileSync('./' + title + '.mp3'));
                 writer.setFrame('TIT2', arama[0].title)
                     .setFrame('TPE1', [arama[0].author.name])
                     .setFrame('APIC', {
                         type: 3,
                         data: fs.readFileSync(title + '.jpg'),
                         description: arama[0].description
                     });
                 writer.addTag();
 
                 reply = await message.client.sendMessage(message.jid, fs.readFileSync('./' + title + '.jpg'), MessageType.image, {caption: '♪ ɴᴀᴍᴇ :  ```' + title + '```\n\n »» [ ᴜᴘʟᴏᴀᴅɪɴɢ ʏᴏᴜʀ sᴏɴɢ ]««\n\n »» [ ᴛʏᴘᴇ: •ᴍᴘ𝟹 & •ᴍʀᴀ ]\n\n' });
                 await message.client.sendMessage(message.jid,Buffer.from(writer.arrayBuffer), MessageType.document, {filename: title + '.mp3', mimetype: 'audio/mpeg', contextInfo: { forwardingScore: 1000, isForwarded: true }, quoted: message.data});
 
                 await message.client.sendMessage(message.jid,Buffer.from(writer.arrayBuffer), MessageType.audio, {mimetype: Mimetype.mp4Audio, contextInfo: { forwardingScore: 1000, isForwarded: true }, quoted: message.data, ptt: false});
             });

            }

            if(regex3.test(message.message)){
     //video

     try {
        var arama = await yts({videoId: ytdl.getURLVideoID(yt_link)});
    } catch {
        return await message.client.sendMessage(message.jid,Lang.NO_RESULT,MessageType.text);
    }

    var reply = await message.client.sendMessage(message.jid,Lang.DOWNLOADING_VIDEO,MessageType.text);

    var yt = ytdl(arama.videoId, {filter: format => format.container === 'mp4' && ['720p', '480p', '360p', '240p', '144p'].map(() => true)});
    yt.pipe(fs.createWriteStream('./' + arama.videoId + '.mp4'));

    yt.on('end', async () => {
        //reply = await message.client.sendMessage(message.jid,Lang.UPLOADING_VIDEO,MessageType.text);
        await message.client.sendMessage(message.jid,fs.readFileSync('./' + arama.videoId + '.mp4'), MessageType.video,{mimetype: Mimetype.mp4, contextInfo: { forwardingScore: 1000, isForwarded: true }, quoted: message.data, caption: arama.title});
    });
            }
        }));
    }));
