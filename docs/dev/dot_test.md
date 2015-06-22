<img src='http://g.gravizo.com/g?
  digraph G {
    label="Wire-cell Data Flow, Components and Status";
    larsoft[shape=box,label="LArSoft"];
    larfile[shape=folder, label="Art ROOT file"];
    lardata[fillcolor=yellow,style=filled,shape=doubleoctagon,label="Chao: MC truth+wire sig\n(needs committing)"];
    largeom[fillcolor=yellow,style=filled,shape=doubleoctagon,label="Chao+Xiaoyue\ngeo dumped from larsoft via RED35 code\n(needs committing)\n(can we directly generate w/out larsoft?)"];
    celltree[shape=folder, label="celltree ROOT file"];
    geomfile_ub[shape=folder, label="geometry text file\n(uboone)"];
    geomfile_dune[shape=folder, label="geometry text files\n(DUNE wrapped wires)"];
    sst[color=darkorchid,shape=component,label="WC SST\nGDS, FDS, SimDS"];
    params[shape=ellipse,label="parameters"];
    nav[color=darkorchid,shape=component,label="WC NAV\nGenerative FrameDataSource (FDS+SimDS)\nParam GeomDataSource\nVarious Hit Depositors"];
    algs[color=darkorchid,shape=component,label="WC algs\n(2dtoy+nav+graph+tiling+...)"];
    xinfile[shape=folder,label="Xin ROOT file"];
    cellio[style=filled,fillcolor=red,shape=doubleoctagon,label="Reco cell I/O\n(needs implementing)"];
    cellfile[shape=folder,label="Reco cell files"];
    root2json[fillcolor=yellow,style=filled,shape=doubleoctagon,label="Chao converter\n(needs committing)"];
    json[shape=folder,label="JSON files"];
    lard[shape=box,label="LAr Data Server\n(lard-server)"];
    disp[style=rounded,shape=trapezium,label="WebGL Visualization"];
    rootvis[style=rounded,shape=trapezium,label="ROOT Visualization"];
    {rank=same; larsoft; params}
    {rank=same; disp; rootvis}
    larsoft->larfile[label="writes"];
    larfile->lardata[style="dashed",label="reads"];
    lardata->celltree[style="dashed",label="writes"];
    largeom->geomfile_ub[style=dashed,label="writes"];
    larsoft->largeom[style=dashed,label="dumps"];
    largeom->geomfile_dune[style=dashed,label="writes"];
    celltree->sst[label="reads"];
    geomfile_ub->sst[label="reads"];
    geomfile_dune->sst[style="dotted",label="reads"];
    sst->algs[label="provides methods"];
    params->nav[label="input"];
    nav->algs[label="provides methods"];
    nav->rootvis;
    algs->cellio[style="dotted",label="writes"];
    cellio->algs[style="dotted",label="reads"];
    cellio->cellfile[style="dotted"];
    cellfile->lard[style="dotted",label="reads"];
    lard->disp[style=dashed, label="HTTP\l(needs common API developed)"];
    celltree->lard[label="reads"];
    larfile->lard[style=dotted, label="raw data + trad. reco\n(needs implementing)"];
    algs->xinfile[label="writes"];
    xinfile->root2json[style=dashed,label="reads"];
    root2json->json[style=dashed,label="writes"];
    json->disp[label="HTTP"];
    algs->rootvis;
}
'/>