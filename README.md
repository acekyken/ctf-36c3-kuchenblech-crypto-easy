# 36c3 junior ctf: easy crypto challenge

Link to this challenge:
<https://github.com/fkt/36c3-junior-ctf-pub/tree/master/chal4>

'''
message = int('REDACTED', base=35)
N = 31882864753733457706900355195561745936205728163688545344755624355885302677527509480805991969514641856022311950710014654686332759895303124949904557581766107448945073828773339824936328117599459705430379854436444155104737774883908742430619368768337640156577480749932446289330171110268995901030116001751822218657
c = message^3 % N
# c = 272712645051843502864020676686837219546440933810920336253597504130258033336636323130656292878088405243095416128

The message is the flag. No flag format.
'''

Ha! This looks familiar. When I saw this challenge I immediately thought of RSA. In RSA, the ciphertext is cipher = m^e % N

We have the ciphertext, which is commented out, and our e is 3. 

First, I opened an interactive python shell and stored N  and c 
'''ipython
>>> N = 31882864753733457706900355195561745936205728163688545344755624355885302677527509480805991969514641856022311950710014654686332759895303124949904557581766107448945073828773339824936328117599459705430379854436444155104737774883908742430619368768337640156577480749932446289330171110268995901030116001751822218657
>>> c = 272712645051843502864020676686837219546440933810920336253597504130258033336636323130656292878088405243095416128
>>> m = c ^ (1/3)
'''


Now, our e is really small - this means that in computing the reverse of m^e=c we get candidates for the correct message ('candidates' because of mod N). If we apply math, we get m= the square root of c = c ^(1/3)

'''ipython
>>> m = c ** (1/3)
>>> m 
6.484877229948686e+36
>>> int(m)
6484877229948686425308087880658190336
'''

Now lets take this result and decode it in base 35. So I tried to find a nice way to convert to base 35 in python. I found this stack overflow answer that did base conversion for strings: 
<https://stackoverflow.com/questions/2267362/how-to-convert-an-integer-in-any-base-to-a-string>
I was way to lazy to find the "correct way" to do this in python. CTFing is not really about doing things nicely, right? It is about finding all the flags asap. So I did not bother with python and took that number to a web converter. 

'''
juniorisseskvokyjqfh46ol
'''

It looks great! the beginning seams correct, and i was not too bothered about the non-standard flag format because the task said that it wouldn't be. But, to my frustration, the flag was not excepted. Ugh. Some issue with the base conversion??


Eventually, I suspected that there was an error with big numbers here.
This number is larger than a standard integer, which is why I think my python solution just did not work out. I cast to int at one point, that was probably the problem. So I ditched python and used wolfram alpha. I raised the number to the power of (1/3) and converted it to base 35.
'''
272712645051843502864020676686837219546440933810920336253597504130258033336636323130656292878088405243095416128^(1/3) to base 35
'''
et voilà: we have a flag!
'''
juniorissmallkuchenblech
'''
What I learned was that I could have done this with wolfram alpha in under 5 minutes. I still don't know what the correct python way is. I tried installing baseconvert via pip and using that, but that gives me an error, probably because it expects integers.
As I already said, CTFing is about finding a tool that works for you as fast as possible, which is why I think it's totally legitimate to use whatever works for you. 
