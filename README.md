# analytics-em-flutter
## Um exemplo de uso do Analytics em Flutter com GetIt


Eu já havia usado o Google Analytics em um app em Flutter, mas achei muito complicado. Com meus conhecimentos limitados de OOP, eu passava via parâmetro o *analytics* e o "observer" a cada troca de página página. Era bem complicado. 

Em um novo aplicativo que fiz (**Meu ônibus SP**: https://pratic.com.br/meu-onibus-sp/) eu deixei de inserir logo de início. Fiquei pesquisando uma forma de fazer que fosse mais elegante e fácil de dar manutenção. Consegui achar um jeito (não sei se é o melhor, mas funciona!) usando o *package* GetIt. 

## O que eu fiz

Criei um arquivo com a classe **AnalyticsService**

'''
import 'package:firebase_analytics/firebase_analytics.dart';
import 'package:firebase_analytics/observer.dart';

class AnalyticsService {
  final FirebaseAnalytics _analytics = FirebaseAnalytics();

  FirebaseAnalyticsObserver getAnalyticsObserver() => FirebaseAnalyticsObserver(analytics: _analytics);

  void sendAnalytics({evento: "geral"}) async {
    await _analytics.logEvent(name: evento);
  }
}
'''

Dentro do **main.dart**, antes de chamar o **runApp(MyApp());** eu faço a chamada da classe:

'''
getIt.registerSingleton<AnalyticsService>(AnalyticsService());
'''
  
Aí, em qualquer página, na montagem do *widget* principal, eu chamo esta classe e envio um evento:

'''
@override
  Widget build(BuildContext context) {
    final analytics = GetIt.I.get<AnalyticsService>();
    analytics.sendAnalytics(evento: 'Recompensa');
'''  

Ou dentro de qualquer lugar, como por exemplo:

'''
onPressed: () {
   analytics.sendAnalytics(evento: 'Nome_evento');
'''                              

## Registro no Analytics

Claro que é preciso ter criado o aplicativo dentro do Analytics para receber os eventos!
