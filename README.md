startermod_locale
=======
해당 저장소는 예제용입니다.

##업데이트:

The overrides method allows any mod to provide a translation for any other mod.

## 주의사항
 - 해당 모드는 한국에 맞게 언어를 재배치하였으며 해당 모드 설치 후 영어로 플레이 시 표기 오류가 생길 수 있습니다. ( 예) Kingdom of [마을이름] -> [마을이름] Kingdom of )
 - manifest.json에서 폰트 로드 시 반드시 폰트를 먼저 오버라이드 한 후에 번역파일을 매치해주세요.
 
##정보
스톤하스 현지화 모드 예제를 사용한 한국어 현지화 모드입니다.

소개 및 사용방법: 모드를 다운로드 하신 후 스톤하스 모드폴더(스팀: C:\Steam\steamapps\common\Stonehearth\mods)에 압축을 주신 후, as a peer to stonehearth.smod and radiant.smod. Make sure the folder is named startermod_locale, not startermod_locale-master.

Inside the startermod_locale folder, you should see a manifest.json file, a added_languages.json file and a translations folder which has 2 folders in it representing the two mods that are shipped by radiant so far (stonehearth and rayyas children). Inside each of those folders will be 2 files named en-XA.json and en-DEV.json.

The startermod_locale mod adds a pseudolocalization of stonehearth's and rayya children's en.json file. The en-DEV.json file is the plain english used to generate en-XA.json. Pseudolocalization replaces all the characters in stonehearth's en.json file with characters that have accents. This mimics what foreign languages will look like when displayed in the game.

When you start up Stonehearth with this mod installed, open the settings menu and go to the system tab. You will see that a language named "[!!Ēņģľĭšħ!!]" will be added to the list of languages. Selecting the "[!!Ēņģľĭšħ!!]" language and clicking "OK" will change the game's language.

##작동방식:

1) startermod_locale's manifest.json uses this line:<br>
   ````
   "mixintos" : {
      "stonehearth/locales/supported_languages.json" : "file(added_languages.json)"
   }
   ````
   This causes all the data in added_languages.json to be added to the list of supported languages recognized by the stonehearth mod.

2) added_languages.json specifies a new language, en-XA like so:<br>
   ````
   "en-XA": {
      "display_name": "[!!Ēņģľĭšħ!!]"
   }
   ````
   display_name is what the settings language drop down will show for the name of the language. You can use this to display the name of a language in its native form.
   the name "en-XA" is the language code of the translation. See: http://www.science.co.il/Language/Locale-codes.asp
   Most languages and language variants have a language code. In this case, en means English and the XA is a made up name for the pseudolocale.
   The German translation will be de-DE.json, and the Chinese Mandarin translation will be zh-CN.json
   Note: If you have a region specific translation, the last 2 letters of the translation file MUST be capitalized!

3) Then, back to startermod_locale's manifest.json, we add the translations provided by this mod using overrides:<br>
   ````
   "overrides": {
      "stonehearth/locales/en-XA.json": "file(translations/stonehearth/en-XA.json)",
      "rayyas_children/locales/en-XA.json": "file(translations/rayyas_children/en-XA.json)"
   }
   ````
   These overrides will effective act as if you added new files under stonehearth/locales/ and rayyas_children/locales/
   The files that are pointed to is the en-XA.json files under the translations folder. This is the file that contains all the translated strings.

##노트:

If you are creating your own content mod that has translatable strings (not a mod that is purely translation), to tell the game that your mod contains locale data, you need to specify a default locale in the mod's manifest.json, ex:
   ````
   {
      "info" : {
      "name" : "rayyas_children",
      "version" : 1
      },
      "default_locale": "en",
   }
   ````

  This tells the game that if the main stonehearth game is running in a language for which your mod doesn't have support, it will know to load the english translation file by default.

##How to translate en.json:

Only translate strings that come after a ':'' in the json file. You can more easily view the json on a website such as http://codebeautify.org/jsonviewer
Do not modify the strings that come on the left side of a ':'
Also, do not modify strings that are surrounded by brackets '[' or ']'

Example:
  ````
  "ai": {
   "actions": {
      "status_text": {
         "chase_entity": "chasing [name(data.target)]",
  ````

Here, the string that needs to be translated is "chasing [name(data.target)]"
Note, DO NOT translate the "[name(data.target)]" portion. This is a special string replacement. The name of the target will replace "[name(data.target)]" in the game.

EX: If a hearthling is chasing a goblin named "Jib", the game will replace "[name(data.target)]" with "Jib" and the final string will read: "chasing Jib"

The replacement is character sensitive, so do not change whether the string is upper or lower case, and do not replace any of the string with a different key. It is best
to copy and past the special portion into your translated text.

You can move "[name(data.target)]" around to different locations within the string.

The japanese translated file might show something like this:<br>
  ````
  "ai": {
   "actions": {
      "status_text": {
         "chase_entity": "[name(data.target)]を追う",
  ````

The Chinese file might look like:<br>
  ````
  "ai": {
   "actions": {
      "status_text": {
         "chase_entity": "追逐[name(data.target)]",
  ````