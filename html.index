import { useState, useCallback, useEffect, useRef } from "react";

const C = { ink:"#1A1714", cream:"#F0F0EE", sand:"#C9B99A", terra:"#8B6B4A", white:"#FAFAFA", blue:"#4A6B8B", green:"#4A7A5A", red:"#8B3A3A", orange:"#B07830" };

const useWidth = () => {
  const [w,setW] = useState(window.innerWidth);
  useEffect(()=>{ const h=()=>setW(window.innerWidth); window.addEventListener("resize",h); return()=>window.removeEventListener("resize",h); },[]);
  return w;
};

const DEFAULT_CV = {
  identite:{ prenom:"Terence", nom:"RICHARD", titre:"Head of Sales · Leader Commercial · Développement et Structuration de Business Units · Grands Comptes B2B · Nouveaux Marchés · France & International", tel:"+33 6 58 84 10 85", email:"terence.richard75@gmail.com", linkedin:"linkedin.com/in/terencerichard", langues:"Français : Langue maternelle · Anglais : Professionnel · Allemand : Courant" },
  profil:"Dirigeant commercial BtoB avec plus de 20 ans d'expérience dans la structuration et le développement de Business Units en contexte de croissance, en France et à l'international.\n\nExpert en définition et déploiement de stratégies commerciales complètes.\n\nExpérience confirmée en management d'équipes via des managers intermédiaires.\n\nReconnu pour identifier et activer de nouveaux leviers de croissance.",
  competences:"STRATÉGIE & DÉVELOPPEMENT COMMERCIAL : Stratégie commerciale B2B · Go-to-market · Développement de comptes stratégiques · Closing deals complexes · Négociation contractuelle · Pilotage RFP\n\nCOMMERCIAL & FINANCIAL PERFORMANCE : Responsabilité P&L · Prévisions CA · Optimisation prix · Gestion des marges\n\nLEADERSHIP & ORGANISATION : Structuration d'organisations commerciales · Management d'équipes internationales\n\nOUTILS : CRM · ChatGPT & IA générative · LinkedIn automation · Airtable",
  experiences:[
    {id:"exta",entreprise:"EXTA ARCHIS",secteur:"Distribution de Mobilier",titre_poste:"Fondateur & Directeur Général",lieu:"France",periode:"2021 - Aujourd'hui",bullets:["Définition de la stratégie commerciale nationale et du positionnement B2B.","Structuration d'un réseau national de clients professionnels.","Développement d'un écosystème de 130 partenaires fournisseurs européens.","Mise en place du pilotage commercial : CRM, pipeline, forecast.","Responsabilité complète du P&L."],resultats:"Transformation réussie du modèle B2C vers B2B · 700+ prescripteurs inscrits · x3 CA · +26% marge brute"},
    {id:"workforce",entreprise:"WORKFORCE LOGIQ",secteur:"VMS & MSP Solution",titre_poste:"Directeur Commercial France",lieu:"France",periode:"2018 - 2020",bullets:["Déploiement stratégique de l'offre sur le marché français.","Ciblage et développement de comptes stratégiques (CAC 40 / SBF 250).","Pilotage de cycles de vente complexes multi-décideurs.","Négociation contractuelle et structuration financière des deals."],resultats:"Signature d'un contrat stratégique de 1M€ · EBITDA positif"},
    {id:"elior",entreprise:"ELIOR FRANCE",secteur:"Restauration d'entreprise",titre_poste:"Directeur des Ventes France",lieu:"Paris",periode:"2016 - 2017",bullets:["Management d'une équipe commerciale nationale (4 FTEs).","Déploiement de la stratégie de développement B2B.","Pilotage de la rentabilité contractuelle."],resultats:"430K€ de CA signé"},
    {id:"bcd",entreprise:"BCD TRAVEL",secteur:"Business Travel",titre_poste:"Directeur Account Management France",lieu:"Paris",periode:"2011 - 2015",bullets:["Direction des équipes commerciales nationales (10 FTEs).","Responsabilité complète du P&L France.","Pilotage stratégie croissance, rétention et upsell.","Conduite d'appels d'offres nationaux et internationaux."],resultats:"+25% de croissance jusqu'à 20M€ de CA · 98% de rétention client · Renégociation des contrats +390K€ EBITDA · 0 départ volontaire"},
    {id:"cwt",entreprise:"CWT",secteur:"Business Travel",titre_poste:"Directeur Comptes Stratégiques Internationaux",lieu:"Paris",periode:"2002 - 2011",bullets:["Gestion de portefeuilles grands comptes nationaux et internationaux.","Responsable du P&L global, stratégie tarifaire et optimisation contractuelle.","Management d'équipes jusqu'à 12 ETP sur 13 pays.","Pilotage RFP internationaux et négociations au niveau exécutif."],resultats:"Portefeuilles grands comptes jusqu'à 6,8M€ · Développement et stabilisation 2,4 à 3M€ sur 13 pays · 100% de rétention client"},
    {id:"hays",entreprise:"HAYS DX",secteur:"Livraison Express",titre_poste:"Responsable des Ventes",lieu:"France",periode:"1999 - 2001",bullets:[],resultats:""},
    {id:"decathlon",entreprise:"DECATHLON",secteur:"Sport & Retail",titre_poste:"Chef de Rayon",lieu:"France",periode:"1997 - 1999",bullets:[],resultats:""},
  ],
  formations:[
    {id:"essec",ecole:"ESSEC Business School",diplome:"Executive Master",specialite:"Strategy & Management of International Business",periode:"2008 - 2010"},
    {id:"dresden",ecole:"HTW Dresden",diplome:"Diplom Kaufmann",specialite:"International Business Studies (Erasmus)",periode:"1995 - 1997"},
    {id:"lehavre",ecole:"Université Le Havre Normandie",diplome:"Maîtrise",specialite:"International Business Studies",periode:"1992 - 1996"},
  ],
  outils:"CRM · Pack Office · Google Suite · Airtable · Notion · LinkedIn automation · ChatGPT & IA générative · Shopify",
  interets:"Photographie · Bricolage · Running",
};

const SYSTEM_PROMPT = `Tu es un expert senior en stratégie de candidature BtoB.

CV SOURCE — TERENCE RICHARD — RÉSULTATS VERBATIM (ne jamais modifier) :
- EXTA ARCHIS (2021-) Fondateur & DG : Transformation réussie du modèle B2C vers B2B · 700+ prescripteurs inscrits · x3 CA · +26% marge brute
- WORKFORCE LOGIQ (2018-2020) Dir Commercial France : Signature d'un contrat stratégique de 1M€ · EBITDA positif
- ELIOR FRANCE (2016-2017) Dir Ventes France : 430K€ de CA signé
- BCD TRAVEL (2011-2015) Dir Account Management France : +25% de croissance jusqu'à 20M€ de CA · 98% de rétention client · Renégociation des contrats +390K€ EBITDA · 0 départ volontaire
- CWT (2002-2011) Dir Comptes Stratégiques Internationaux : Portefeuilles grands comptes jusqu'à 6,8M€ · Développement et stabilisation 2,4 à 3M€ sur 13 pays · 100% de rétention client
- FORMATIONS : ESSEC Executive Master (2008-2010) · HTW Dresden Erasmus (1995-1997) · Univ. Le Havre Maîtrise (1992-1996)

RÈGLES ABSOLUES :
1. Résultats chiffrés reproduits VERBATIM. Jamais modifiés.
2. Aucune expérience, chiffre ou diplôme inventé.
3. Réponse en français sauf si langue = EN.
4. Chaque régénération différente de la précédente.`;

const callClaude = async (prompt) => {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method:"POST",
    headers:{"Content-Type":"application/json"},
    body:JSON.stringify({
      model:"claude-sonnet-4-20250514",
      max_tokens:1500,
      system:SYSTEM_PROMPT,
      messages:[{role:"user",content:prompt}]
    })
  });
  if(!res.ok){ const e=await res.json().catch(()=>({})); throw new Error(e.error?.message||"Erreur API "+res.status); }
  const d=await res.json();
  return d.content?.[0]?.text?.trim()||"";
};

const store = {
  getCVBase:()=>{ try{const d=localStorage.getItem("cv_base");return d?JSON.parse(d):DEFAULT_CV;}catch{return DEFAULT_CV;} },
  saveCVBase:(cv)=>{ try{localStorage.setItem("cv_base",JSON.stringify(cv));}catch{} },
  getCandidatures:()=>{ try{const d=localStorage.getItem("candidatures");return d?JSON.parse(d):[];}catch{return[];} },
  saveCandidatures:(list)=>{ try{localStorage.setItem("candidatures",JSON.stringify(list));}catch{} },
};

const mkCand = (form) => ({
  id:"c_"+Date.now(), createdAt:new Date().toISOString(), finaliseeAt:"",
  type:form.type, entreprise:form.entreprise, poste:form.poste,
  langue:form.langue||"FR", annonce:form.annonce||"", briefEntreprise:form.briefEntreprise||"",
  statut:"brouillon", analyseAnnonce:"", scoreFit:0, angleStrategique:"", goNogo:"",
  titreAdapte:"", titreVarianteChoisie:"", profilAdapte:"", competencesAdaptees:"",
  experiencesAdaptees:"", varianteConservative:"", varianteBalanced:"", varianteAggressive:"",
  varianteChoisie:"", lettre:"", guideEntretien:"",
  etapes:{analyse:"idle",titre:"idle",profil:"idle",competences:"idle",experiences:"idle",variantes:"idle",lettre:"idle",guide:"idle"},
  notes:"",
});

const buildCtx = (c) => {
  let s="CANDIDATURE : "+c.poste+" chez "+c.entreprise+" ("+(c.type==="A"?"sur annonce":"spontanée")+", "+c.langue+")\n";
  if(c.annonce) s+="\nANNONCE :\n"+c.annonce+"\n";
  if(c.briefEntreprise) s+="\nBRIEF ENTREPRISE :\n"+c.briefEntreprise+"\n";
  if(c.analyseAnnonce) s+="\nANALYSE VALIDÉE :\n"+c.analyseAnnonce+"\n";
  if(c.titreAdapte) s+="\nTITRE VALIDÉ : "+c.titreAdapte+"\n";
  if(c.profilAdapte) s+="\nPROFIL VALIDÉ :\n"+c.profilAdapte+"\n";
  if(c.competencesAdaptees) s+="\nCOMPÉTENCES VALIDÉES :\n"+c.competencesAdaptees+"\n";
  if(c.experiencesAdaptees) s+="\nEXPÉRIENCES VALIDÉES :\n"+c.experiencesAdaptees+"\n";
  if(c.varianteChoisie) s+="\nVARIANTE RETENUE :\n"+c.varianteChoisie+"\n";
  return s;
};

const NR = (p) => p?"VERSION PRÉCÉDENTE À NE PAS RÉPÉTER :\n"+p+"\n\nGénère quelque chose de DIFFÉRENT.\n":"";

const PROMPTS = {
  analyse:(c)=>buildCtx(c)+"\nAnalyse cette "+(c.type==="A"?"annonce":"opportunité spontanée")+" pour Terence RICHARD.\n\nFormat EXACT :\n\nSCORE DE FIT\n[0-100] — [justification]\n\nENJEUX CLÉS\n- [Enjeu 1]\n- [Enjeu 2]\n- [Enjeu 3]\n\nATTENTES IMPLICITES DU RECRUTEUR\n- [Attente 1]\n- [Attente 2]\n\nANGLE STRATÉGIQUE RECOMMANDÉ\n[Comment positionner Terence RICHARD]\n\nPOINTS FORTS À METTRE EN AVANT\n- [Point 1 avec référence au CV]\n- [Point 2]\n\nPOINTS DE VIGILANCE\n- [Point 1]\n\nRECOMMANDATION GO / NO GO\n[GO ou NO GO] — [Justification]",
  titre:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nGénère 3 variantes de titre.\n\nTITRE CONSERVATIVE\n[légèrement adapté]\n\nTITRE BALANCED\n[rééquilibré vers les mots-clés]\n\nTITRE AGGRESSIVE\n[fortement repositionné]\n\nPOURQUOI CES CHOIX\n[justification courte]",
  profil:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nGénère un profil adapté.\n\nPROFIL ADAPTÉ\n[3-4 phrases. Résultats chiffrés VERBATIM.]\n\nCE QUI A CHANGÉ\n- [Changement 1]\n\nCE QUI EST CONSERVÉ VERBATIM\n- [Élément avec chiffres exacts]",
  competences:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nSélectionne et réorganise les compétences.\n\nCOMPÉTENCES PRIORITAIRES POUR CE POSTE\n[sélection réorganisée]\n\nCOMPÉTENCES À DÉPRIORISER\n[liste]\n\nJUSTIFICATION\n[pourquoi]",
  experiences:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nDétermine l'ordre et la mise en avant des expériences.\n\nEXPÉRIENCES À METTRE EN AVANT (ordre recommandé)\n\n1. [ENTREPRISE] — [POSTE] ([PÉRIODE])\n   Missions : [mission adaptée]\n   Résultat verbatim : [résultat EXACT du CV]\n\n2. [ENTREPRISE] — [POSTE]\n   Missions : [mission]\n   Résultat verbatim : [résultat EXACT]\n\nEXPÉRIENCES À PASSER EN SECOND PLAN\n- [liste]\n\nJUSTIFICATION\n[pourquoi]",
  variantes:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nGénère 3 variantes de positionnement global.\n\nVARIANTE CONSERVATIVE\nPhilosophie : proche du CV original\nTitre : [titre]\nAccroche : [phrase]\nCe qui change : [points]\n\nVARIANTE BALANCED\nPhilosophie : équilibre identité et adaptation\nTitre : [titre]\nAccroche : [phrase]\nCe qui change : [points]\n\nVARIANTE AGGRESSIVE\nPhilosophie : repositionnement fort\nTitre : [titre]\nAccroche : [phrase]\nCe qui change : [points]",
  lettre:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nRédige une lettre de motivation complète pour "+c.poste+" chez "+c.entreprise+". Date : "+new Date().toLocaleDateString("fr-FR")+". Preuves chiffrées VERBATIM. Signer Terence RICHARD.",
  guide:(c,p)=>buildCtx(c)+"\n"+NR(p)+"\nGénère un guide de préparation à l'entretien.\n\nQUESTIONS PROBABLES\n1. [Question] → Réponse : [...]\n2. [Question] → Réponse : [...]\n3. [Question] → Réponse : [...]\n\nPOINTS FORTS À METTRE EN AVANT\n- [Point avec preuve chiffrée]\n\nPOINTS DE VIGILANCE\n- [Point] → Traitement : [...]\n\nQUESTIONS À POSER AU RECRUTEUR\n- [Question 1]\n- [Question 2]\n\nPOSTURE RECOMMANDÉE\n[Ton et style recommandés]",
  brief:(e,p)=>"Génère un brief stratégique pour une candidature spontanée chez "+e+" pour un poste de "+p+".\n\nSECTEUR & ACTIVITÉ\n[description]\n\nENJEUX ACTUELS 2024-2025\n[défis identifiés]\n\nSIGNAUX DE CROISSANCE\n[recrutements, levées]\n\nOPPORTUNITÉ POUR TERENCE RICHARD\n[pourquoi ce profil]\n\nANGLE DE CANDIDATURE\n[comment le positionner]",
};

const ETAPE_ORDER=["analyse","titre","profil","competences","experiences","variantes","lettre","guide"];
const ETAPE_LABELS={analyse:"Analyse",titre:"Titre",profil:"Profil",competences:"Compétences",experiences:"Expériences",variantes:"Variantes",lettre:"Lettre",guide:"Entretien"};
const ETAPE_FIELDS={analyse:"analyseAnnonce",titre:"titreAdapte",profil:"profilAdapte",competences:"competencesAdaptees",experiences:"experiencesAdaptees",variantes:"varianteChoisie",lettre:"lettre",guide:"guideEntretien"};
const STATUTS={brouillon:{label:"Brouillon",color:"rgba(26,23,20,0.35)"},en_cours:{label:"En cours",color:"#4A6B8B"},a_finaliser:{label:"À finaliser",color:"#B07830"},finalisee:{label:"Finalisée",color:"#4A7A5A"},archive:{label:"Archivée",color:"rgba(26,23,20,0.25)"}};

const exportCSV=(cand)=>{const esc=(v)=>'"'+String(v||"").replace(/"/g,'""')+'"';const rows=[["champ","valeur"],...Object.entries(cand).map(([k,v])=>[k,typeof v==="object"?JSON.stringify(v):v])];const csv="\uFEFF"+rows.map((r)=>r.map(esc).join(",")).join("\r\n");const a=document.createElement("a");a.href=URL.createObjectURL(new Blob([csv],{type:"text/csv;charset=utf-8;"}));a.download="candidature_"+cand.entreprise.replace(/\s+/g,"_")+"_"+cand.id+".csv";document.body.appendChild(a);a.click();document.body.removeChild(a);};
const importCSV=(text)=>{const lines=text.split(/\r?\n/).filter((l)=>l.trim()&&!l.startsWith('"champ"'));const obj={};for(const line of lines){const m=line.match(/^"([^"]+)","((?:[^"]|"")*)"$/);if(m){try{obj[m[1]]=JSON.parse(m[2].replace(/""/g,'"'));}catch{obj[m[1]]=m[2].replace(/""/g,'"');}}}if(!obj.id)return null;return{...mkCand({type:obj.type||"A",entreprise:obj.entreprise||"",poste:obj.poste||"",langue:obj.langue||"FR"}),...obj};};
const exportText=(fn,c)=>{const a=document.createElement("a");a.href=URL.createObjectURL(new Blob([c],{type:"text/plain;charset=utf-8"}));a.download=fn;document.body.appendChild(a);a.click();document.body.removeChild(a);};
const exportCV=(cand,cvBase)=>{const id=cvBase.identite;const sep="\n"+"─".repeat(50)+"\n";let t=id.prenom+" "+id.nom+"\n"+(cand.titreAdapte||id.titre)+"\n"+id.tel+" | "+id.email+"\n"+id.langues;t+=sep+"PROFIL"+sep+(cand.profilAdapte||cvBase.profil);t+=sep+"COMPÉTENCES"+sep+(cand.competencesAdaptees||cvBase.competences);t+=sep+"EXPÉRIENCES"+sep;if(cand.experiencesAdaptees){t+=cand.experiencesAdaptees;}else{cvBase.experiences.forEach((e)=>{t+=e.entreprise+" — "+e.titre_poste+" | "+e.periode+"\n";e.bullets.forEach((b)=>{if(b)t+="• "+b+"\n";});if(e.resultats)t+="Résultats : "+e.resultats+"\n";t+="\n";});}t+=sep+"FORMATION"+sep;cvBase.formations.forEach((f)=>{t+=f.ecole+" — "+f.diplome+" | "+f.periode+"\n";});t+="\nOUTILS : "+cvBase.outils;exportText("CV_"+id.nom+"_"+cand.entreprise.replace(/\s+/g,"_")+".txt",t);};
const exportDossier=(cand)=>{const sep="\n"+"═".repeat(60)+"\n";let t="DOSSIER — "+cand.entreprise+" | "+cand.poste+sep+(cand.analyseAnnonce||"—")+sep+"TITRE : "+(cand.titreAdapte||"—")+sep+"PROFIL :\n"+(cand.profilAdapte||"—")+sep+"COMPÉTENCES :\n"+(cand.competencesAdaptees||"—")+sep+"EXPÉRIENCES :\n"+(cand.experiencesAdaptees||"—")+sep+"VARIANTE :\n"+(cand.varianteChoisie||"—")+sep+"LETTRE :\n"+(cand.lettre||"—")+sep+"GUIDE :\n"+(cand.guideEntretien||"—");exportText("Dossier_"+cand.entreprise.replace(/\s+/g,"_")+".txt",t);};

// ─── UI primitives ────────────────────────────────────────────────
const Lbl=({c=C.terra,children})=><div style={{fontSize:"9px",letterSpacing:"0.22em",textTransform:"uppercase",color:c,marginBottom:"6px",fontFamily:"Georgia, serif"}}>{children}</div>;
const Div=()=><div style={{width:"36px",height:"1px",background:C.sand,margin:"10px 0 14px"}}/>;
const Field=({value,onChange,placeholder="",multiline=false,rows=4,readOnly=false})=>{const s={width:"100%",padding:"10px 12px",border:"1px solid rgba(201,185,154,"+(readOnly?"0.25":"0.45")+")",background:readOnly?"rgba(201,185,154,0.05)":C.white,fontFamily:"Georgia, serif",fontSize:"13px",color:C.ink,outline:"none",lineHeight:"1.7",marginBottom:"10px",boxSizing:"border-box",borderRadius:0};return multiline?<textarea value={value} onChange={(e)=>!readOnly&&onChange&&onChange(e.target.value)} rows={rows} placeholder={placeholder} readOnly={readOnly} style={{...s,resize:readOnly?"none":"vertical"}}/>:<input value={value} onChange={(e)=>!readOnly&&onChange&&onChange(e.target.value)} placeholder={placeholder} readOnly={readOnly} style={s}/>;};
const Btn=({children,onClick,variant="primary",disabled=false,full=false,small=false})=>{const V={primary:[C.ink,C.cream,C.ink],terra:[C.terra,C.cream,C.terra],ghost:["transparent","rgba(26,23,20,0.55)","rgba(201,185,154,0.4)"],danger:["transparent",C.red,C.red],add:["transparent",C.terra,C.terra],success:[C.green,C.cream,C.green],blue:[C.blue,C.cream,C.blue],warning:[C.orange,C.cream,C.orange]};const[bg,col,brd]=V[variant]||V.primary;return<button onClick={onClick} disabled={disabled} style={{padding:small?"6px 12px":"10px 20px",fontSize:small?"9px":"11px",letterSpacing:"0.14em",textTransform:"uppercase",fontFamily:"Georgia, serif",background:disabled?"rgba(26,23,20,0.08)":bg,color:disabled?"rgba(26,23,20,0.25)":col,border:"1px solid "+(disabled?"rgba(201,185,154,0.2)":brd),cursor:disabled?"not-allowed":"pointer",borderRadius:0,width:full?"100%":"auto"}}>{children}</button>;};
const Modal=({title,body,buttons})=><div style={{position:"fixed",inset:0,background:"rgba(26,23,20,0.75)",zIndex:9999,display:"flex",alignItems:"center",justifyContent:"center",padding:"20px"}}><div style={{background:C.cream,padding:"28px 32px",maxWidth:"480px",width:"100%"}}><h3 style={{fontFamily:"Georgia, serif",fontSize:"18px",fontWeight:"300",marginBottom:"14px"}}>{title}</h3><div style={{fontSize:"13px",lineHeight:"1.85",color:"rgba(26,23,20,0.7)",fontFamily:"Georgia, serif",marginBottom:"20px",whiteSpace:"pre-line"}}>{body}</div><div style={{display:"flex",gap:"8px",flexWrap:"wrap"}}>{buttons}</div></div></div>;

// ─── TOP NAV ─────────────────────────────────────────────────────
const TopNav=({screen,setScreen})=>{const w=useWidth();const mob=w<640;return<div style={{position:"fixed",top:0,left:0,right:0,zIndex:1000,height:"52px",background:C.ink,display:"flex",alignItems:"center",justifyContent:"space-between",padding:"0 20px",borderBottom:"1px solid rgba(201,185,154,0.12)"}}><div style={{display:"flex",alignItems:"center"}}><span style={{fontFamily:"Georgia, serif",fontSize:"13px",fontWeight:"300",color:C.cream,marginRight:"24px"}}>{mob?"Candidatures":"Système de Candidature — Terence RICHARD"}</span>{!mob&&[["dashboard","Candidatures"],["cvbase","CV de base"]].map(([id,l])=><button key={id} onClick={()=>setScreen(id)} style={{padding:"0 16px",height:"52px",fontSize:"10px",letterSpacing:"0.14em",textTransform:"uppercase",fontFamily:"Georgia, serif",background:"transparent",color:screen===id?C.cream:"rgba(253,250,245,0.38)",border:"none",borderBottom:screen===id?"2px solid "+C.sand:"2px solid transparent",cursor:"pointer"}}>{l}</button>)}</div>{mob&&<select value={screen} onChange={(e)=>setScreen(e.target.value)} style={{padding:"6px 10px",background:C.ink,color:C.cream,border:"1px solid rgba(201,185,154,0.3)",fontFamily:"Georgia, serif",fontSize:"10px",borderRadius:0}}><option value="dashboard">Candidatures</option><option value="cvbase">CV de base</option></select>}</div>;};

// ─── DASHBOARD ───────────────────────────────────────────────────
const Dashboard=({candidatures,setCandidatures,onOpen,onNew})=>{const w=useWidth();const mob=w<640;const fileRef=useRef();
  const handleImport=(e)=>{const file=e.target.files?.[0];if(!file)return;const reader=new FileReader();reader.onload=(ev)=>{const cand=importCSV(ev.target.result);if(!cand){alert("CSV invalide.");return;}const idx=candidatures.findIndex((c)=>c.id===cand.id);const next=idx>=0?candidatures.map((c,i)=>i===idx?cand:c):[cand,...candidatures];setCandidatures(next);store.saveCandidatures(next);onOpen(cand.id);};reader.readAsText(file,"utf-8");e.target.value="";};
  const del=(id)=>{if(!window.confirm("Supprimer ?"))return;const next=candidatures.filter((c)=>c.id!==id);setCandidatures(next);store.saveCandidatures(next);};
  return<div style={{marginTop:"52px",padding:mob?"22px 14px":"40px 44px",background:C.cream,minHeight:"calc(100vh - 52px)"}}>
    <input ref={fileRef} type="file" accept=".csv" style={{display:"none"}} onChange={handleImport}/>
    <div style={{display:"flex",justifyContent:"space-between",alignItems:mob?"flex-start":"flex-end",marginBottom:"28px",flexDirection:mob?"column":"row",gap:mob?"14px":"0"}}>
      <div><Lbl>Tableau de bord</Lbl><h1 style={{fontFamily:"Georgia, serif",fontSize:mob?"26px":"32px",fontWeight:"300"}}>Mes <em style={{color:C.terra,fontStyle:"italic"}}>candidatures</em></h1><Div/></div>
      <div style={{display:"flex",gap:"8px",flexWrap:"wrap"}}><Btn onClick={()=>fileRef.current?.click()} variant="ghost" small>↑ Importer CSV</Btn><Btn onClick={onNew} variant="terra">+ Nouvelle candidature</Btn></div>
    </div>
    {candidatures.length===0?<div style={{background:C.white,padding:"52px 44px",textAlign:"center"}}><div style={{fontFamily:"Georgia, serif",fontSize:"18px",fontWeight:"300",color:"rgba(26,23,20,0.4)",marginBottom:"16px"}}>Aucune candidature</div><Btn onClick={onNew} variant="terra">Créer ma première candidature</Btn></div>:
    <div style={{display:"flex",flexDirection:"column",gap:"3px"}}>
      {candidatures.map((c)=>{const st=STATUTS[c.statut]||STATUTS.brouillon;return<div key={c.id} style={{background:C.white,padding:mob?"14px":"18px 24px",display:"grid",gridTemplateColumns:"1fr auto",gap:"14px",alignItems:"center",borderLeft:"3px solid "+st.color,cursor:"pointer"}} onClick={()=>onOpen(c.id)}>
        <div><div style={{fontFamily:"Georgia, serif",fontSize:mob?"14px":"16px",fontWeight:"400",marginBottom:"3px"}}>{c.entreprise} — {c.poste}</div><div style={{fontSize:"10px",color:"rgba(26,23,20,0.45)"}}>{c.type==="A"?"Sur annonce":"Spontanée"} · {c.langue} · {c.createdAt?new Date(c.createdAt).toLocaleDateString("fr-FR"):""}{c.scoreFit?" · Score : "+c.scoreFit+"/100":""}</div></div>
        <div style={{display:"flex",gap:"6px",alignItems:"center"}}><span style={{fontSize:"9px",letterSpacing:"0.14em",textTransform:"uppercase",color:st.color,border:"1px solid "+st.color,padding:"3px 8px",whiteSpace:"nowrap"}}>{st.label}</span><button onClick={(e)=>{e.stopPropagation();exportCSV(c);}} style={{padding:"4px 8px",background:"transparent",border:"1px solid rgba(201,185,154,0.4)",color:"rgba(26,23,20,0.45)",cursor:"pointer",fontSize:"9px",borderRadius:0}}>↓ CSV</button><button onClick={(e)=>{e.stopPropagation();del(c.id);}} style={{padding:"4px 8px",background:"transparent",border:"1px solid rgba(139,58,58,0.28)",color:C.red,cursor:"pointer",fontSize:"10px",borderRadius:0}}>✕</button></div>
      </div>;})}
    </div>}
  </div>;};

// ─── NEW CANDIDATURE ─────────────────────────────────────────────
const NewCandidature=({onCreate,onCancel})=>{const w=useWidth();const mob=w<640;const[type,setType]=useState(null);const[form,setForm]=useState({entreprise:"",poste:"",langue:"FR",annonce:"",briefEntreprise:""});const[loadingBrief,setLoadingBrief]=useState(false);const[errBrief,setErrBrief]=useState("");const up=(k,v)=>setForm((p)=>({...p,[k]:v}));
  const genBrief=async()=>{if(!form.entreprise||!form.poste)return;setLoadingBrief(true);setErrBrief("");try{const b=await callClaude(PROMPTS.brief(form.entreprise,form.poste));up("briefEntreprise",b);}catch(e){setErrBrief("Erreur : "+e.message);}setLoadingBrief(false);};
  if(!type)return<div style={{marginTop:"52px",padding:mob?"22px 14px":"40px 44px",background:C.cream,minHeight:"calc(100vh - 52px)"}}><div style={{maxWidth:"720px"}}><Lbl>Nouvelle candidature</Lbl><h1 style={{fontFamily:"Georgia, serif",fontSize:mob?"22px":"28px",fontWeight:"300",marginBottom:"6px"}}>Quel type de <em style={{color:C.terra,fontStyle:"italic"}}>candidature ?</em></h1><Div/><div style={{display:"grid",gridTemplateColumns:mob?"1fr":"1fr 1fr",gap:"3px"}}>{[{t:"A",title:"A — Réponse à une annonce",desc:"Tu as trouvé une offre. Tu colles l'annonce et le système analyse tout.",color:C.terra},{t:"D",title:"D — Candidature spontanée",desc:"Tu as identifié une entreprise sans annonce. Le système génère un brief.",color:C.blue}].map((opt)=><div key={opt.t} onClick={()=>setType(opt.t)} style={{background:C.white,padding:"28px 24px",cursor:"pointer",borderTop:"3px solid "+opt.color}}><div style={{fontFamily:"Georgia, serif",fontSize:"18px",fontWeight:"400",marginBottom:"10px",color:opt.color}}>{opt.title}</div><p style={{fontSize:"12px",color:"rgba(26,23,20,0.55)",fontFamily:"Georgia, serif",lineHeight:"1.75"}}>{opt.desc}</p></div>)}</div><div style={{marginTop:"20px"}}><Btn onClick={onCancel} variant="ghost">Annuler</Btn></div></div></div>;
  const canCreate=form.entreprise&&form.poste&&(type==="A"?form.annonce:form.briefEntreprise);
  return<div style={{marginTop:"52px",padding:mob?"22px 14px":"40px 44px",background:C.cream,minHeight:"calc(100vh - 52px)"}}><div style={{maxWidth:"720px"}}>
    <Lbl>Nouvelle candidature — {type==="A"?"Sur annonce":"Spontanée"}</Lbl>
    <h1 style={{fontFamily:"Georgia, serif",fontSize:mob?"22px":"26px",fontWeight:"300",marginBottom:"6px"}}>{type==="A"?<span>Coller <em style={{color:C.terra,fontStyle:"italic"}}>l'annonce</em></span>:<span>Analyser <em style={{color:C.blue,fontStyle:"italic"}}>l'entreprise</em></span>}</h1><Div/>
    <div style={{display:"grid",gridTemplateColumns:mob?"1fr":"1fr 1fr",gap:mob?"0":"0 18px"}}>
      <div><Lbl>Entreprise</Lbl><Field value={form.entreprise} onChange={(v)=>up("entreprise",v)} placeholder="Ex : Semactic"/></div>
      <div><Lbl>Poste visé</Lbl><Field value={form.poste} onChange={(v)=>up("poste",v)} placeholder="Ex : Country Manager France"/></div>
      <div><Lbl>Langue</Lbl><select value={form.langue} onChange={(e)=>up("langue",e.target.value)} style={{width:"100%",padding:"10px 12px",border:"1px solid rgba(201,185,154,0.45)",background:C.white,fontFamily:"Georgia, serif",fontSize:"13px",outline:"none",marginBottom:"10px",borderRadius:0}}><option value="FR">FR — Français</option><option value="EN">EN — English</option></select></div>
    </div>
    {type==="A"&&<><Lbl>Texte complet de l'annonce</Lbl><Field value={form.annonce} onChange={(v)=>up("annonce",v)} placeholder="Colle ici le texte de l'annonce..." multiline rows={mob?8:12}/></>}
    {type==="D"&&<><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:"6px"}}><Lbl>Brief entreprise</Lbl><Btn onClick={genBrief} disabled={!form.entreprise||!form.poste||loadingBrief} variant="blue" small>{loadingBrief?"Analyse...":"✦ Analyser avec Claude"}</Btn></div>
    {errBrief&&<div style={{padding:"10px",background:"rgba(139,58,58,0.06)",border:"1px solid "+C.red,color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{errBrief}</div>}
    {!form.briefEntreprise&&!loadingBrief&&<div style={{background:C.white,border:"1px solid rgba(201,185,154,0.3)",padding:"20px",marginBottom:"10px",fontFamily:"Georgia, serif",fontSize:"13px",color:"rgba(26,23,20,0.4)",fontStyle:"italic"}}>Remplis l'entreprise et le poste, puis clique "Analyser avec Claude".</div>}
    {form.briefEntreprise&&<Field value={form.briefEntreprise} onChange={(v)=>up("briefEntreprise",v)} multiline rows={mob?10:14}/>}</>}
    <div style={{display:"flex",gap:"10px",marginTop:"8px",flexWrap:"wrap"}}><Btn onClick={()=>onCreate(mkCand({...form,type}))} disabled={!canCreate} variant="terra">Créer et analyser →</Btn><Btn onClick={()=>setType(null)} variant="ghost">← Retour</Btn></div>
  </div></div>;};

// ─── CV BASE ─────────────────────────────────────────────────────
const CVBase=({cvBase,setCVBase})=>{const w=useWidth();const mob=w<640;const[cv,setCV]=useState(cvBase);const[sec,setSec]=useState("identite");const[saved,setSaved]=useState(false);
  const save=()=>{store.saveCVBase(cv);setCVBase(cv);setSaved(true);setTimeout(()=>setSaved(false),2800);};
  const upId=(f,v)=>setCV((p)=>({...p,identite:{...p.identite,[f]:v}}));
  const upExp=(id,e)=>setCV((p)=>({...p,experiences:p.experiences.map((x)=>x.id===id?e:x)}));
  const nav=[{id:"identite",l:"01 Identité"},{id:"profil",l:"02 Profil"},{id:"competences",l:"03 Compétences"},{id:"experiences",l:"04 Expériences"},{id:"formations",l:"05 Formations"},{id:"outils",l:"06 Outils"}];
  const g2={display:"grid",gridTemplateColumns:mob?"1fr":"1fr 1fr",gap:mob?"0":"0 18px"};
  return<div style={{display:"flex",marginTop:"52px",minHeight:"calc(100vh - 52px)"}}>
    {!mob&&<div style={{width:"185px",background:C.ink,flexShrink:0,position:"sticky",top:"52px",height:"calc(100vh - 52px)",overflowY:"auto",display:"flex",flexDirection:"column",justifyContent:"space-between"}}><div style={{padding:"20px 0 8px"}}>{nav.map((it)=><button key={it.id} onClick={()=>setSec(it.id)} style={{display:"block",width:"100%",padding:"11px 18px",background:sec===it.id?"rgba(201,185,154,0.1)":"transparent",borderLeft:sec===it.id?"2px solid "+C.sand:"2px solid transparent",color:sec===it.id?C.cream:"rgba(253,250,245,0.38)",fontSize:"10px",letterSpacing:"0.14em",textTransform:"uppercase",fontFamily:"Georgia, serif",border:"none",cursor:"pointer",textAlign:"left"}}>{it.l}</button>)}</div><div style={{padding:"16px"}}><button onClick={save} style={{width:"100%",padding:"9px",background:saved?C.green:C.terra,color:C.cream,border:"none",fontSize:"9px",letterSpacing:"0.18em",textTransform:"uppercase",fontFamily:"Georgia, serif",cursor:"pointer"}}>{saved?"✓ Sauvegardé":"Sauvegarder"}</button></div></div>}
    {mob&&<div style={{position:"fixed",top:"52px",left:0,right:0,zIndex:200,background:C.ink,display:"flex",overflowX:"auto",borderBottom:"1px solid rgba(201,185,154,0.12)"}}>{nav.map((it)=><button key={it.id} onClick={()=>setSec(it.id)} style={{flexShrink:0,padding:"11px 13px",fontSize:"9px",letterSpacing:"0.13em",textTransform:"uppercase",fontFamily:"Georgia, serif",background:"transparent",border:"none",borderBottom:sec===it.id?"2px solid "+C.sand:"2px solid transparent",color:sec===it.id?C.cream:"rgba(253,250,245,0.38)",cursor:"pointer",whiteSpace:"nowrap"}}>{it.l}</button>)}</div>}
    <div style={{flex:1,padding:mob?"18px 14px":"32px 44px",marginTop:mob?"38px":"0",background:C.cream,overflowY:"auto"}}>
      <div style={{marginBottom:"24px"}}><Lbl>CV de base — source unique</Lbl><h1 style={{fontFamily:"Georgia, serif",fontSize:mob?"22px":"26px",fontWeight:"300"}}>Ton profil <em style={{color:C.terra,fontStyle:"italic"}}>complet</em></h1><Div/></div>
      {sec==="identite"&&<div style={g2}><div><Lbl>Prénom</Lbl><Field value={cv.identite.prenom} onChange={(v)=>upId("prenom",v)}/></div><div><Lbl>Nom</Lbl><Field value={cv.identite.nom} onChange={(v)=>upId("nom",v)}/></div><div style={{gridColumn:mob?"1":"1/-1"}}><Lbl>Titre principal</Lbl><Field value={cv.identite.titre} onChange={(v)=>upId("titre",v)}/></div><div><Lbl>Téléphone</Lbl><Field value={cv.identite.tel} onChange={(v)=>upId("tel",v)}/></div><div><Lbl>Email</Lbl><Field value={cv.identite.email} onChange={(v)=>upId("email",v)}/></div><div style={{gridColumn:mob?"1":"1/-1"}}><Lbl>LinkedIn</Lbl><Field value={cv.identite.linkedin} onChange={(v)=>upId("linkedin",v)}/></div><div style={{gridColumn:mob?"1":"1/-1"}}><Lbl>Langues</Lbl><Field value={cv.identite.langues} onChange={(v)=>upId("langues",v)}/></div></div>}
      {sec==="profil"&&<><Lbl>Profil</Lbl><Field value={cv.profil} onChange={(v)=>setCV((p)=>({...p,profil:v}))} multiline rows={mob?8:12}/></>}
      {sec==="competences"&&<><Lbl>Compétences clés</Lbl><Field value={cv.competences} onChange={(v)=>setCV((p)=>({...p,competences:v}))} multiline rows={mob?10:14}/></>}
      {sec==="experiences"&&<div>{cv.experiences.map((exp,i)=>(<div key={exp.id} style={{background:C.white,border:"1px solid rgba(201,185,154,0.28)",padding:mob?"14px":"20px",marginBottom:"12px"}}><div style={{fontFamily:"Georgia, serif",fontSize:"13px",marginBottom:"10px"}}>{exp.entreprise||"Exp. "+(i+1)}</div><div style={g2}><div><Lbl>Entreprise</Lbl><Field value={exp.entreprise} onChange={(v)=>upExp(exp.id,{...exp,entreprise:v})}/></div><div><Lbl>Période</Lbl><Field value={exp.periode} onChange={(v)=>upExp(exp.id,{...exp,periode:v})}/></div><div style={{gridColumn:mob?"1":"1/-1"}}><Lbl>Titre du poste</Lbl><Field value={exp.titre_poste} onChange={(v)=>upExp(exp.id,{...exp,titre_poste:v})}/></div></div><Lbl>Résultats — verbatim</Lbl><Field value={exp.resultats} onChange={(v)=>upExp(exp.id,{...exp,resultats:v})} multiline rows={2}/></div>))}</div>}
      {sec==="formations"&&<div>{cv.formations.map((f)=>(<div key={f.id} style={{background:C.white,border:"1px solid rgba(201,185,154,0.28)",padding:"14px",marginBottom:"10px"}}><div style={g2}><div><Lbl>École</Lbl><Field value={f.ecole} onChange={(v)=>setCV((p)=>({...p,formations:p.formations.map((x)=>x.id===f.id?{...f,ecole:v}:x)}))}/></div><div><Lbl>Diplôme</Lbl><Field value={f.diplome} onChange={(v)=>setCV((p)=>({...p,formations:p.formations.map((x)=>x.id===f.id?{...f,diplome:v}:x)}))}/></div><div style={{gridColumn:mob?"1":"1/-1"}}><Lbl>Période</Lbl><Field value={f.periode} onChange={(v)=>setCV((p)=>({...p,formations:p.formations.map((x)=>x.id===f.id?{...f,periode:v}:x)}))}/></div></div></div>))}</div>}
      {sec==="outils"&&<><Lbl>Outils</Lbl><Field value={cv.outils} onChange={(v)=>setCV((p)=>({...p,outils:v}))} multiline rows={3}/><Lbl>Intérêts</Lbl><Field value={cv.interets} onChange={(v)=>setCV((p)=>({...p,interets:v}))} multiline rows={2}/></>}
      {mob&&<div style={{marginTop:"20px"}}><Btn onClick={save} variant={saved?"success":"terra"} full>{saved?"✓ Sauvegardé":"Sauvegarder"}</Btn></div>}
    </div>
  </div>;};

// ─── ETAPE BLOCK ─────────────────────────────────────────────────
const EtapeBlock=({label,contenu,setContenu,onRegenerer,onValider,statut,loading,error,readOnly})=>{const[local,setLocal]=useState(contenu||"");const[editing,setEditing]=useState(false);useEffect(()=>{setLocal(contenu||"");},[contenu]);
  if(statut==="validee"&&!editing)return<div style={{background:C.white,border:"1px solid rgba(201,185,154,0.22)",marginBottom:"12px"}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",padding:"10px 14px",background:"rgba(74,122,90,0.06)",borderBottom:"1px solid rgba(74,122,90,0.15)"}}><span style={{fontSize:"9px",letterSpacing:"0.14em",textTransform:"uppercase",color:C.green,fontFamily:"Georgia, serif"}}>✓ {label} — Validé</span>{!readOnly&&<button onClick={()=>setEditing(true)} style={{fontSize:"9px",color:"rgba(26,23,20,0.45)",background:"transparent",border:"none",cursor:"pointer",fontFamily:"Georgia, serif"}}>✏️ Modifier</button>}</div><div style={{padding:"14px",fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.8",color:C.ink,maxHeight:"100px",overflow:"hidden"}}>{(contenu||"").substring(0,200)}{(contenu||"").length>200?"...":""}</div></div>;
  return<div style={{background:C.white,border:"1px solid "+(statut==="a_regenerer"?C.orange:"rgba(201,185,154,0.22)"),marginBottom:"16px"}}>
    <div style={{padding:"10px 14px",background:statut==="a_regenerer"?"rgba(176,120,48,0.08)":"rgba(201,185,154,0.05)",borderBottom:"1px solid rgba(201,185,154,0.15)",display:"flex",justifyContent:"space-between",alignItems:"center"}}><span style={{fontSize:"9px",letterSpacing:"0.14em",textTransform:"uppercase",color:statut==="a_regenerer"?C.orange:C.terra,fontFamily:"Georgia, serif"}}>{statut==="a_regenerer"?"⚠️ À régénérer — ":""}{label}</span>{loading&&<span style={{fontSize:"10px",color:C.terra,fontFamily:"Georgia, serif",fontStyle:"italic"}}>⟳ Génération en cours...</span>}</div>
    <div style={{padding:"14px"}}>
      {error&&<div style={{padding:"10px",background:"rgba(139,58,58,0.06)",border:"1px solid "+C.red,color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{error}</div>}
      <textarea value={editing?local:(contenu||"")} onChange={(e)=>editing?setLocal(e.target.value):setContenu&&setContenu(e.target.value)} rows={12} readOnly={!editing&&readOnly} style={{width:"100%",padding:"10px",border:"1px solid rgba(201,185,154,0.25)",background:C.white,fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.82",outline:"none",resize:"vertical",borderRadius:0,boxSizing:"border-box",color:C.ink}}/>
      {!readOnly&&<div style={{display:"flex",gap:"8px",marginTop:"10px",flexWrap:"wrap"}}>
        {onRegenerer&&<Btn onClick={onRegenerer} disabled={loading} variant="ghost" small>↺ Régénérer</Btn>}
        {editing?<><Btn onClick={()=>{setContenu&&setContenu(local);setEditing(false);}} variant="terra" small>✓ Appliquer</Btn><Btn onClick={()=>{setLocal(contenu||"");setEditing(false);}} variant="ghost" small>Annuler</Btn></>:onValider&&<Btn onClick={onValider} disabled={loading||!contenu} variant="success" small>✓ Valider cette étape</Btn>}
      </div>}
    </div>
  </div>;};

// ─── CANDIDATURE DETAIL ──────────────────────────────────────────
const CandidatureDetail=({candidature:initCand,candidatures,setCandidatures,onBack,cvBase})=>{const w=useWidth();const mob=w<640;const[cand,setCandLocal]=useState(initCand);const[activeTab,setActiveTab]=useState("analyse");const[loading,setLoading]=useState({});const[errors,setErrors]=useState({});const[modal,setModal]=useState(null);const[finalizeConfirm,setFinalizeConfirm]=useState(false);const[modifyWarning,setModifyWarning]=useState(null);const isReadOnly=cand.statut==="finalisee";const analyseDone=useRef(false);
  const updateCand=useCallback((updates)=>{const next={...cand,...updates};setCandLocal(next);const all=store.getCandidatures();store.saveCandidatures(all.map((c)=>c.id===next.id?next:c));setCandidatures(all.map((c)=>c.id===next.id?next:c));},[cand,setCandidatures]);
  const generer=async(etape)=>{const field=ETAPE_FIELDS[etape];const promptFn=PROMPTS[etape];setLoading((p)=>({...p,[etape]:true}));setErrors((p)=>({...p,[etape]:""}));updateCand({etapes:{...cand.etapes,[etape]:"loading"},statut:"en_cours"});try{const prev=cand[field]||"";const result=await callClaude(promptFn(cand,prev));const updates={[field]:result,etapes:{...cand.etapes,[etape]:"idle"},statut:"en_cours"};if(etape==="analyse"){const m1=result.match(/SCORE DE FIT\s*\n(\d+)/i);if(m1)updates.scoreFit=parseInt(m1[1]);const m2=result.match(/GO\s*\/\s*NO GO\s*\n(GO|NO GO)/i);if(m2)updates.goNogo=m2[1];}if(etape==="variantes"){const consv=result.match(/VARIANTE CONSERVATIVE\s*([\s\S]+?)(?=VARIANTE BALANCED|$)/i)?.[1]?.trim()||"";const bal=result.match(/VARIANTE BALANCED\s*([\s\S]+?)(?=VARIANTE AGGRESSIVE|$)/i)?.[1]?.trim()||"";const agg=result.match(/VARIANTE AGGRESSIVE\s*([\s\S]+?)$/i)?.[1]?.trim()||"";updates.varianteConservative=consv;updates.varianteBalanced=bal;updates.varianteAggressive=agg;updates.varianteChoisie="";delete updates[field];}updateCand(updates);}catch(e){setErrors((p)=>({...p,[etape]:"Erreur : "+e.message}));updateCand({etapes:{...cand.etapes,[etape]:"error"}});}setLoading((p)=>({...p,[etape]:false}));};
  const validerEtape=(etape,override)=>{const field=ETAPE_FIELDS[etape];const val=override!==undefined?override:cand[field];const idx=ETAPE_ORDER.indexOf(etape);const updates={[field]:val,etapes:{...cand.etapes,[etape]:"validee"}};if(idx===ETAPE_ORDER.length-1)updates.statut="a_finaliser";updateCand(updates);if(idx<ETAPE_ORDER.length-1)setTimeout(()=>setActiveTab(ETAPE_ORDER[idx+1]),300);};
  const demanderModification=(etape)=>{const idx=ETAPE_ORDER.indexOf(etape);const suivantes=ETAPE_ORDER.slice(idx+1).filter((e)=>cand.etapes[e]==="validee");if(suivantes.length>0)setModifyWarning({etape,suivantes});else updateCand({etapes:{...cand.etapes,[etape]:"idle"}});};
  const confirmerModification=(etape)=>{const idx=ETAPE_ORDER.indexOf(etape);const newE={...cand.etapes,[etape]:"idle"};ETAPE_ORDER.slice(idx+1).forEach((e)=>{newE[e]="a_regenerer";});updateCand({etapes:newE});setModifyWarning(null);setActiveTab(etape);};
  const finaliser=()=>{updateCand({statut:"finalisee",finaliseeAt:new Date().toISOString()});setTimeout(()=>{exportCV(cand,cvBase);exportDossier(cand);exportCSV(cand);},200);setFinalizeConfirm(false);};
  useEffect(()=>{if(!analyseDone.current&&cand.etapes.analyse==="idle"&&!cand.analyseAnnonce){analyseDone.current=true;generer("analyse");}},[]);
  const allValidees=ETAPE_ORDER.every((e)=>cand.etapes[e]==="validee");
  const stCoul=(STATUTS[cand.statut]||STATUTS.brouillon).color;
  const TABS=[{id:"recapitulatif",l:"10 · Récap"},{id:"analyse",l:"2 · Analyse"},{id:"titre",l:"3 · Titre"},{id:"profil",l:"4 · Profil"},{id:"competences",l:"5 · Compét."},{id:"experiences",l:"6 · Expér."},{id:"variantes",l:"7 · Variantes"},{id:"lettre",l:"8 · Lettre"},{id:"guide",l:"9 · Entretien"}];
  const DepWarn=({dep})=>cand.etapes[dep]!=="validee"?<div style={{padding:"10px 14px",background:"rgba(176,120,48,0.08)",border:"1px solid "+C.orange,marginBottom:"16px",fontSize:"12px",fontFamily:"Georgia, serif",color:C.orange}}>⚠️ Valider d'abord "{ETAPE_LABELS[dep]}" avant de continuer.</div>:null;
  const BtnGen=({etape,dep,label})=>{const ok=!dep||cand.etapes[dep]==="validee";return<div style={{textAlign:"center",padding:"32px"}}><Btn onClick={()=>generer(etape)} disabled={!ok||loading[etape]} variant="terra">{loading[etape]?"Génération en cours...":"✦ "+label}</Btn></div>;};
  const renderStd=(etape,dep,btnLabel)=>{const field=ETAPE_FIELDS[etape];return<div>{dep&&<DepWarn dep={dep}/>}{cand.etapes[etape]==="validee"&&!loading[etape]&&<><EtapeBlock label={ETAPE_LABELS[etape]+" validé(e)"} contenu={cand[field]} setContenu={()=>{}} statut="validee" readOnly={isReadOnly}/>{!isReadOnly&&<Btn onClick={()=>demanderModification(etape)} variant="ghost" small>✏️ Modifier</Btn>}</>}{cand.etapes[etape]!=="validee"&&<>{!cand[field]&&!loading[etape]&&<BtnGen etape={etape} dep={dep} label={btnLabel}/>}{loading[etape]&&<div style={{padding:"20px",fontFamily:"Georgia, serif",fontSize:"13px",color:C.terra,fontStyle:"italic"}}>Génération en cours avec Claude...</div>}{errors[etape]&&<div style={{color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{errors[etape]}</div>}{cand[field]&&!loading[etape]&&!isReadOnly&&<EtapeBlock label={ETAPE_LABELS[etape]+" généré(e)"} contenu={cand[field]} setContenu={(v)=>updateCand({[field]:v})} onRegenerer={()=>generer(etape)} onValider={()=>validerEtape(etape)} statut={cand.etapes[etape]} loading={loading[etape]} error={errors[etape]} readOnly={isReadOnly}/>}</div>};

  return<div style={{marginTop:"52px",minHeight:"calc(100vh - 52px)",background:C.cream}}>
    {modifyWarning&&<Modal title="⚠️ Modifier cette étape ?" body={"Modifier \""+ETAPE_LABELS[modifyWarning.etape]+"\" invalidera :\n"+modifyWarning.suivantes.map((e)=>"• "+ETAPE_LABELS[e]).join("\n")} buttons={[<Btn key="c" onClick={()=>setModifyWarning(null)} variant="ghost">Annuler</Btn>,<Btn key="o" onClick={()=>confirmerModification(modifyWarning.etape)} variant="warning">Modifier quand même →</Btn>]}/>}
    {finalizeConfirm&&<Modal title="⚠️ Finaliser cette candidature ?" body={"Une fois finalisée :\n• Vous ne pourrez plus modifier les contenus.\n• Votre CV adapté sera exporté.\n• Votre dossier complet sera exporté.\n• Un fichier CSV sera téléchargé.\n\nCette action est irréversible."} buttons={[<Btn key="c" onClick={()=>setFinalizeConfirm(false)} variant="ghost">Annuler</Btn>,<Btn key="o" onClick={finaliser} variant="success">Oui, finaliser →</Btn>]}/>}
    {modal==="nogo"&&<Modal title="NO GO — Que faire ?" body="Cette opportunité ne semble pas correspondre. Quelle action ?" buttons={[<Btn key="del" onClick={()=>{const all=store.getCandidatures().filter((c)=>c.id!==cand.id);store.saveCandidatures(all);setCandidatures(all);onBack();}} variant="danger" small>Supprimer</Btn>,<Btn key="arch" onClick={()=>{updateCand({statut:"archive",goNogo:"NOGO"});setModal(null);onBack();}} variant="ghost" small>Archiver</Btn>,<Btn key="cont" onClick={()=>{updateCand({goNogo:"NOGO"});validerEtape("analyse");setModal(null);}} variant="warning" small>Continuer quand même</Btn>,<Btn key="ann" onClick={()=>setModal(null)} variant="ghost" small>Annuler</Btn>]}/>}

    <div style={{background:C.ink,padding:mob?"12px 14px 0":"14px 24px 0",borderBottom:"1px solid rgba(201,185,154,0.1)"}}>
      <div style={{display:"flex",alignItems:"center",gap:"10px",marginBottom:"10px",flexWrap:"wrap"}}>
        <button onClick={onBack} style={{fontSize:"10px",letterSpacing:"0.12em",textTransform:"uppercase",color:"rgba(201,185,154,0.5)",background:"none",border:"none",cursor:"pointer",fontFamily:"Georgia, serif"}}>← Retour</button>
        <span style={{fontFamily:"Georgia, serif",fontSize:mob?"13px":"15px",fontWeight:"300",color:C.cream}}>{cand.entreprise} — {cand.poste}</span>
        <span style={{fontSize:"9px",color:stCoul,border:"1px solid "+stCoul,padding:"2px 7px",fontFamily:"Georgia, serif"}}>{(STATUTS[cand.statut]||STATUTS.brouillon).label}</span>
        {cand.scoreFit>0&&<span style={{fontSize:"9px",color:C.sand,fontFamily:"Georgia, serif"}}>Score : {cand.scoreFit}/100</span>}
        {isReadOnly&&<span style={{fontSize:"9px",color:C.green,fontFamily:"Georgia, serif"}}>🔒 Finalisée</span>}
      </div>
      <div style={{display:"flex",overflowX:"auto"}}>{TABS.map((t)=>{const etape=t.id==="recapitulatif"?null:t.id;const est=etape?cand.etapes[etape]:null;const isDone=est==="validee";const isRegen=est==="a_regenerer";const isActive=activeTab===t.id;return<button key={t.id} onClick={()=>setActiveTab(t.id)} style={{flexShrink:0,padding:"8px 12px",fontSize:"9px",letterSpacing:"0.1em",textTransform:"uppercase",fontFamily:"Georgia, serif",background:"transparent",color:isActive?C.cream:isDone?C.green:isRegen?C.orange:"rgba(253,250,245,0.35)",border:"none",borderBottom:isActive?"2px solid "+C.sand:isDone?"2px solid "+C.green:isRegen?"2px solid "+C.orange:"2px solid transparent",cursor:"pointer",whiteSpace:"nowrap"}}>{isDone?"✓ ":isRegen?"⚠ ":""}{t.l}</button>;})}</div>
    </div>

    <div style={{padding:mob?"16px 14px":"24px 36px",maxWidth:"920px"}}>
      {activeTab==="analyse"&&<div><Lbl>Étape 2 — Analyse initiale</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Analyse de <em style={{color:C.terra,fontStyle:"italic"}}>l'opportunité</em></h2><Div/>
        <div style={{background:C.white,padding:"12px 16px",marginBottom:"16px",borderLeft:"3px solid "+C.terra}}><Lbl>{cand.type==="A"?"Annonce source":"Brief entreprise"}</Lbl><div style={{fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.75",color:"rgba(26,23,20,0.65)",maxHeight:"100px",overflowY:"auto",whiteSpace:"pre-wrap"}}>{cand.type==="A"?cand.annonce:cand.briefEntreprise}</div></div>
        {loading.analyse&&!cand.analyseAnnonce&&<div style={{padding:"24px",background:C.white,textAlign:"center",fontFamily:"Georgia, serif",fontSize:"13px",color:C.terra,fontStyle:"italic"}}>⟳ Analyse en cours avec Claude...</div>}
        {errors.analyse&&<div style={{color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{errors.analyse}</div>}
        {cand.analyseAnnonce&&<EtapeBlock label="Analyse Claude" contenu={cand.analyseAnnonce} setContenu={(v)=>updateCand({analyseAnnonce:v})} onRegenerer={()=>generer("analyse")} onValider={null} statut={cand.etapes.analyse} loading={loading.analyse} error={errors.analyse} readOnly={isReadOnly}/>}
        {cand.analyseAnnonce&&cand.etapes.analyse!=="validee"&&!isReadOnly&&<div style={{display:"flex",gap:"8px",marginBottom:"16px"}}><Btn onClick={()=>{updateCand({goNogo:"GO"});validerEtape("analyse");}} variant="success">✓ GO — Continuer</Btn><Btn onClick={()=>setModal("nogo")} variant="danger">✕ NO GO</Btn></div>}
      </div>}

      {activeTab==="titre"&&<div><Lbl>Étape 3 — Titre adapté</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Titre <em style={{color:C.terra,fontStyle:"italic"}}>adapté</em></h2><Div/>
        <DepWarn dep="analyse"/>
        {cand.etapes.titre==="validee"&&!loading.titre&&<><EtapeBlock label="Titre validé" contenu={cand.titreAdapte} setContenu={()=>{}} statut="validee" readOnly={isReadOnly}/>{!isReadOnly&&<Btn onClick={()=>demanderModification("titre")} variant="ghost" small>✏️ Modifier</Btn>}</>}
        {cand.etapes.titre!=="validee"&&<>
          {!cand.titreAdapte&&!loading.titre&&<BtnGen etape="titre" dep="analyse" label="Générer 3 variantes de titre"/>}
          {loading.titre&&<div style={{padding:"20px",fontFamily:"Georgia, serif",fontSize:"13px",color:C.terra,fontStyle:"italic"}}>Génération des variantes avec Claude...</div>}
          {errors.titre&&<div style={{color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{errors.titre}</div>}
          {cand.titreAdapte&&!loading.titre&&<>
            {["CONSERVATIVE","BALANCED","AGGRESSIVE"].map((type)=>{const rx=new RegExp("TITRE "+type+"\\s*\\n([^\\n]+)","i");const m=cand.titreAdapte.match(rx);const titre=m?m[1].trim():null;if(!titre)return null;const sel=cand.titreVarianteChoisie===type.toLowerCase();return<div key={type} style={{background:C.white,padding:"14px 18px",marginBottom:"3px",borderLeft:sel?"3px solid "+C.green:"3px solid transparent"}}><div style={{fontSize:"9px",letterSpacing:"0.14em",textTransform:"uppercase",color:C.terra,marginBottom:"5px",fontFamily:"Georgia, serif"}}>Variante {type}</div><div style={{fontFamily:"Georgia, serif",fontSize:"14px",color:C.ink,marginBottom:"8px"}}>{titre}</div>{!isReadOnly&&<Btn onClick={()=>updateCand({titreAdapte:titre,titreVarianteChoisie:type.toLowerCase()})} variant={sel?"success":"ghost"} small>{sel?"✓ Sélectionné":"Choisir"}</Btn>}</div>;})}
            <div style={{padding:"14px",background:"rgba(201,185,154,0.06)",marginTop:"8px",marginBottom:"12px"}}><Lbl>Texte complet — modifiable</Lbl><textarea value={cand.titreAdapte} onChange={(e)=>!isReadOnly&&updateCand({titreAdapte:e.target.value})} rows={8} readOnly={isReadOnly} style={{width:"100%",padding:"10px",border:"1px solid rgba(201,185,154,0.3)",background:C.white,fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.8",outline:"none",resize:"vertical",borderRadius:0,boxSizing:"border-box",color:C.ink}}/></div>
            {!isReadOnly&&<div style={{display:"flex",gap:"8px"}}><Btn onClick={()=>generer("titre")} variant="ghost" small>↺ Régénérer</Btn><Btn onClick={()=>validerEtape("titre")} disabled={!cand.titreAdapte} variant="success" small>✓ Valider le titre</Btn></div>}
          </>}
        </>}
      </div>}

      {activeTab==="profil"&&<div><Lbl>Étape 4 — Profil adapté</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Profil <em style={{color:C.terra,fontStyle:"italic"}}>adapté</em></h2><Div/>{renderStd("profil","titre","Générer le profil adapté")}</div>}
      {activeTab==="competences"&&<div><Lbl>Étape 5 — Compétences</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Compétences <em style={{color:C.terra,fontStyle:"italic"}}>sélectionnées</em></h2><Div/>{renderStd("competences","profil","Générer les compétences adaptées")}</div>}
      {activeTab==="experiences"&&<div><Lbl>Étape 6 — Expériences</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Expériences <em style={{color:C.terra,fontStyle:"italic"}}>valorisées</em></h2><Div/>{renderStd("experiences","competences","Générer les expériences adaptées")}</div>}

      {activeTab==="variantes"&&<div><Lbl>Étape 7 — Variantes globales</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>3 variantes de <em style={{color:C.terra,fontStyle:"italic"}}>positionnement</em></h2><Div/>
        <DepWarn dep="experiences"/>
        {cand.etapes.variantes==="validee"&&!loading.variantes&&<><EtapeBlock label="Variante validée" contenu={cand.varianteChoisie} setContenu={()=>{}} statut="validee" readOnly={isReadOnly}/>{!isReadOnly&&<Btn onClick={()=>demanderModification("variantes")} variant="ghost" small>✏️ Modifier</Btn>}</>}
        {cand.etapes.variantes!=="validee"&&<>
          {!cand.varianteConservative&&!loading.variantes&&<BtnGen etape="variantes" dep="experiences" label="Générer les 3 variantes"/>}
          {loading.variantes&&<div style={{padding:"20px",fontFamily:"Georgia, serif",fontSize:"13px",color:C.terra,fontStyle:"italic"}}>Génération des variantes avec Claude...</div>}
          {errors.variantes&&<div style={{color:C.red,fontSize:"12px",fontFamily:"Georgia, serif",marginBottom:"10px"}}>{errors.variantes}</div>}
          {cand.varianteConservative&&!loading.variantes&&<>
            {[{key:"varianteConservative",label:"Conservative",color:C.blue},{key:"varianteBalanced",label:"Balanced",color:C.terra},{key:"varianteAggressive",label:"Aggressive",color:C.red}].map(({key,label,color})=>{const sel=cand.varianteChoisie===cand[key];return<div key={key} style={{background:C.white,marginBottom:"3px",border:sel?"2px solid "+C.green:"1px solid rgba(201,185,154,0.22)"}}><div style={{padding:"10px 14px",display:"flex",justifyContent:"space-between",alignItems:"center",background:"rgba(201,185,154,0.05)",borderBottom:"1px solid rgba(201,185,154,0.15)"}}><span style={{fontSize:"9px",letterSpacing:"0.14em",textTransform:"uppercase",color,fontFamily:"Georgia, serif"}}>Variante {label}</span>{!isReadOnly&&<Btn onClick={()=>updateCand({varianteChoisie:cand[key]})} variant={sel?"success":"ghost"} small>{sel?"✓ Sélectionnée":"Choisir"}</Btn>}</div><div style={{padding:"12px 14px",fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.8",color:C.ink,whiteSpace:"pre-wrap"}}>{cand[key]}</div></div>;}) }
            <div style={{padding:"14px",background:"rgba(201,185,154,0.06)",marginTop:"8px",marginBottom:"12px"}}><Lbl>✏️ Composer ma version (libre)</Lbl><textarea value={cand.varianteChoisie||""} onChange={(e)=>!isReadOnly&&updateCand({varianteChoisie:e.target.value})} rows={6} placeholder="Mélange les 3 variantes ou écris ta propre version..." readOnly={isReadOnly} style={{width:"100%",padding:"10px",border:"1px solid rgba(201,185,154,0.3)",background:C.white,fontFamily:"Georgia, serif",fontSize:"12px",lineHeight:"1.82",outline:"none",resize:"vertical",borderRadius:0,boxSizing:"border-box",color:C.ink}}/></div>
            {!isReadOnly&&<div style={{display:"flex",gap:"8px"}}><Btn onClick={()=>generer("variantes")} variant="ghost" small>↺ Régénérer</Btn><Btn onClick={()=>validerEtape("variantes")} disabled={!cand.varianteChoisie} variant="success" small>✓ Valider la variante choisie</Btn></div>}
          </>}
        </>}
      </div>}

      {activeTab==="lettre"&&<div><Lbl>Étape 8 — Lettre de motivation</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Lettre de <em style={{color:C.terra,fontStyle:"italic"}}>motivation</em></h2><Div/>{renderStd("lettre","variantes","Générer la lettre de motivation")}</div>}
      {activeTab==="guide"&&<div><Lbl>Étape 9 — Guide de préparation</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Préparation <em style={{color:C.terra,fontStyle:"italic"}}>entretien</em></h2><Div/>{renderStd("guide","lettre","Générer le guide d'entretien")}</div>}

      {activeTab==="recapitulatif"&&<div style={{maxWidth:"800px"}}>
        <Lbl>Étape 10 — Récapitulatif final</Lbl><h2 style={{fontFamily:"Georgia, serif",fontSize:mob?"20px":"24px",fontWeight:"300",marginBottom:"4px"}}>Dossier <em style={{color:C.terra,fontStyle:"italic"}}>complet</em></h2><Div/>
        {isReadOnly&&<div style={{background:"rgba(74,122,90,0.08)",border:"1px solid "+C.green,padding:"16px 18px",marginBottom:"20px"}}><div style={{fontSize:"12px",color:C.green,fontFamily:"Georgia, serif",marginBottom:"12px"}}>✓ Candidature finalisée le {new Date(cand.finaliseeAt).toLocaleDateString("fr-FR")}<br/>🔒 Verrouillée.</div><div style={{display:"flex",gap:"8px",flexWrap:"wrap"}}><Btn onClick={()=>exportCV(cand,cvBase)} variant="success" small>↓ CV adapté</Btn><Btn onClick={()=>exportDossier(cand)} variant="success" small>↓ Dossier complet</Btn><Btn onClick={()=>exportCSV(cand)} variant="ghost" small>↓ CSV</Btn></div></div>}
        <div style={{display:"flex",flexDirection:"column",gap:"3px",marginBottom:"20px"}}>
          {[{e:"analyse",l:"Analyse",p:cand.scoreFit?"Score : "+cand.scoreFit+"/100 — "+cand.goNogo:""},{e:"titre",l:"Titre adapté",p:(cand.titreAdapte||"").substring(0,70)},{e:"profil",l:"Profil adapté",p:(cand.profilAdapte||"").substring(0,70)},{e:"competences",l:"Compétences",p:(cand.competencesAdaptees||"").substring(0,70)},{e:"experiences",l:"Expériences",p:(cand.experiencesAdaptees||"").substring(0,70)},{e:"variantes",l:"Variante choisie",p:(cand.varianteChoisie||"").substring(0,70)},{e:"lettre",l:"Lettre",p:(cand.lettre||"").substring(0,70)},{e:"guide",l:"Guide entretien",p:(cand.guideEntretien||"").substring(0,70)}].map(({e,l,p})=>{const isDone=cand.etapes[e]==="validee";return<div key={e} style={{background:C.white,padding:"12px 16px",display:"flex",justifyContent:"space-between",alignItems:"center",borderLeft:"3px solid "+(isDone?C.green:"rgba(201,185,154,0.3)")}}>
            <div><div style={{fontSize:"11px",color:isDone?C.ink:"rgba(26,23,20,0.45)",fontFamily:"Georgia, serif"}}>{isDone?"✓ ":"○ "}{l}</div>{isDone&&p&&<div style={{fontSize:"10px",color:"rgba(26,23,20,0.45)",fontFamily:"Georgia, serif",marginTop:"2px"}}>{p}{p.length>=70?"...":""}</div>}</div>
            {!isDone&&!isReadOnly&&<Btn onClick={()=>setActiveTab(e)} variant="ghost" small>→ Aller</Btn>}
          </div>;})}
        </div>
        <div style={{marginBottom:"20px"}}><Lbl>Notes libres</Lbl><Field value={cand.notes||""} onChange={(v)=>updateCand({notes:v})} multiline rows={3} placeholder="Tes notes sur cette candidature..." readOnly={isReadOnly}/></div>
        {!isReadOnly&&(allValidees?<div style={{background:"rgba(74,122,90,0.06)",border:"1px solid "+C.green,padding:"20px"}}><div style={{fontSize:"13px",color:C.ink,fontFamily:"Georgia, serif",marginBottom:"12px"}}>✓ Toutes les étapes sont validées. Ton dossier est prêt.</div><Btn onClick={()=>setFinalizeConfirm(true)} variant="success">⚠️ Finaliser ma candidature →</Btn></div>:<div style={{background:"rgba(201,185,154,0.08)",border:"1px solid rgba(201,185,154,0.3)",padding:"16px"}}><div style={{fontSize:"12px",color:"rgba(26,23,20,0.55)",fontFamily:"Georgia, serif"}}>{ETAPE_ORDER.filter((e)=>cand.etapes[e]!=="validee").length} étape(s) restante(s) avant de finaliser.</div></div>)}
      </div>}
    </div>
  </div>;};

// ─── APP ROOT ────────────────────────────────────────────────────
export default function App() {
  const [cvBase,setCVBase]=useState(()=>store.getCVBase());
  const [candidatures,setCandidatures]=useState(()=>store.getCandidatures());
  const [screen,setScreen]=useState("dashboard");
  const [activeCandId,setActiveCandId]=useState(null);
  const [creating,setCreating]=useState(false);
  const saveCands=useCallback((list)=>{setCandidatures(list);store.saveCandidatures(list);},[]);
  const activeCand=candidatures.find((c)=>c.id===activeCandId);
  const goScreen=(s)=>{setScreen(s);setCreating(false);setActiveCandId(null);};
  const handleCreate=(c)=>{saveCands([c,...candidatures]);setActiveCandId(c.id);setCreating(false);};
  return<div style={{fontFamily:"Georgia, serif",color:C.ink,background:C.cream,minHeight:"100vh"}}>
    <TopNav screen={screen} setScreen={goScreen}/>
    {screen==="cvbase"&&<CVBase cvBase={cvBase} setCVBase={(cv)=>{setCVBase(cv);store.saveCVBase(cv);}}/>}
    {screen==="dashboard"&&!creating&&!activeCandId&&<Dashboard candidatures={candidatures} setCandidatures={saveCands} onOpen={setActiveCandId} onNew={()=>setCreating(true)}/>}
    {screen==="dashboard"&&creating&&<NewCandidature onCreate={handleCreate} onCancel={()=>setCreating(false)}/>}
    {screen==="dashboard"&&activeCandId&&activeCand&&<CandidatureDetail candidature={activeCand} candidatures={candidatures} setCandidatures={saveCands} onBack={()=>setActiveCandId(null)} cvBase={cvBase}/>}
  </div>;
}
