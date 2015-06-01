# lucet #

Wiring Compiler

## Generating example JSON files ##

- `utils/ld_fig14a_json_gen topo.json`
- `./utils/appfest_gen -out appfest.json -physical_ports 10 -physical_hosts 4 -ofp_ports 4 -virtual_hosts 1`

The defaults values are:

```erlang
{out, "output.json"}, %% JSON output filename
{physical_hosts, 2}, %% physical hosts
{physical_ports, 4}, %% physical ports per ph
{ofp_ports, 4}, %% OpenFlow ports per vh with LNICX
{virtual_hosts, 1} %% virtual hosts per ph apart from vh with LINCX
```

## Running the compiler ##

### Start Lucet and Dobby
Start the Lucet Wiring Compile and import the topoloy file that will
be imported into a local Dobby server that will be brought up along with Lucet:

`make run topo=lucet_design_fig_17_A.json`

Alternativley, you can connect to an existing dobby using:

`make connect`

In that case, you must import the topology into dobby using the dobby shell:

```erlang
dby_bulk:import(json, ".../lucet_design_fig_17_A.json").
```

And on the lucet shell you must install the lucet search functions into dobby before doing any operations:

```erlang
dby:install(lucet).
```

### Check topoilogy import

Verify that the topology is imported:

```erlang
3> dby:links(<<"PH1">>).
[{<<"PH1/PP1">>,
  #{<<"type">> => #{publisher_id => <<"lucet">>,
      timestamp => <<"2015-05-26T16:22:02Z">>,
      value => <<"part_of">>}}}]
```

### Create wires

Wire `PH1/VH1/OFS1/OFP1` with `PH2/VH1/OFS1/OFP1`:

```erlang
lucet:wire2(<<"PH1/VH1/OFS1/OFP1">>, <<"PH2/VH1/OFS1/OFP1">>).
```

Wire `PH1/VH1/OFS1/OFP2` with `PH1/VH2/EP1` and `PH2/VH1/OFS1/OFP2` with `PH2/VH2/EP1`:

```erlang
lucet:wire2(<<"PH1/VH1/OFS1/OFP2">>, <<"PH1/VH2/EP1">>).
lucet:wire2(<<"PH2/VH1/OFS1/OFP2">>, <<"PH2/VH2/EP1">>).
```

### Verify wires

Verify that the `bound_to` path exists between wired identifiers:

```erlang
lucet:get_bound_to_path(<<"PH1/VH1/OFS1/OFP2">>, <<"PH1/VH2/EP1">>).
```

